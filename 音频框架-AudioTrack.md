# 1. 概述

关键类位置：

| 名称                 | 路径                                                 |
| -------------------- | ---------------------------------------------------- |
| audioserver.rc       | frameworks/av/media/audioserver/audioserver.rc       |
| main_audioserver.cpp | frameworks/av/media/audioserver/main_audioserver.cpp |
| processState.cpp     | frameworks/native/libs/binder                        |
| IServiceManager.cpp  | frameworks/native/libs/binder                        |
| AudioTrack.cpp       | frameworks/av/media/libaudioclient                   |
|                      |                                                      |
|                      |                                                      |
|                      |                                                      |
|                      |                                                      |

关键so/exe/jar源文件位置：

| 名称                     | 路径                                                 |
| ------------------------ | ---------------------------------------------------- |
| audioserver.exe          | frameworks/av/media/audioserver/                     |
| libaudioservice.so       |                                                      |
| libaudioflinger.so       | frameworks/av/services/audioflinger/                 |
| libaudiopolicyservice.so | frameworks/av/services/audiopolicy/services/service/ |
| libaudioclient.so        | frameworks/av/media/libaudioclient/                  |

audioserver依赖的共享库：

![image-20210921102707057](.\images\image-20210921102707057.png)

音频框架图：

![](.\images\音频框架图.png)

# 2. AudioTrack的JNI部分

# 3. MediaPlayer到AudioTrack

这一节重点梳理下应用层调用MediaPlayer后，是如何一步步创建AudioTrack的。

## 3.1 应用层代码

简单的使用MediaPlayer的播放流程，播放assets下的音频文件。

```java
package com.colin.mediaplayerdemo;

import ...

public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    private Button mPlay;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initView();
    }

    private void initView() {
        mPlay = findViewById(R.id.play);
        mPlay.setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.play:
                try {
                    MediaPlayer mp = new MediaPlayer();
                    mp.setOnPreparedListener(mediaPlayer -> mediaPlayer.start());
                    AssetManager assetMg = this.getApplicationContext().getAssets();
                    AssetFileDescriptor fileDescriptor = null;
                    fileDescriptor = assetMg.openFd("music.mp3");
                    mp.setDataSource(fileDescriptor.getFileDescriptor(), fileDescriptor.getStartOffset(), fileDescriptor.getLength());
                    mp.prepare();
                } catch (IOException e) {
                    e.printStackTrace();
                }
                break;
            default:
                break;
        }
    }
}
```

## 3.2 MediaPlayer的调用流程

简单看下MediaPlayer的调用流程

![image-20210921132855725](.\images\image-20210921132855725.png)

实际上，不仅仅是MediaPlayer，其他很多地方也是类似的。上面通过jni透传到native的就不关注了，主要关注下binder的部分。

## 3.3 setDataSource

setDataSource为例，看下mediaplayer.cpp是怎么调用服务侧的：

> mediaplayer

```c++
status_t MediaPlayer::setDataSource(int fd, int64_t offset, int64_t length)
{
    ALOGV("setDataSource(%d, %" PRId64 ", %" PRId64 ")", fd, offset, length);
    status_t err = UNKNOWN_ERROR;
    // 获取IMediaPlayerService，实际上是BpMediaPlayerService
    const sp<IMediaPlayerService> service(getMediaPlayerService());
    if (service != 0) {
        // 调用BpMediaPlayerService的函数，返回获取IMediaPlayer,实际上拿到的是BpMediaPlayer
        sp<IMediaPlayer> player(service->create(this, mAudioSessionId, mOpPackageName));
        if ((NO_ERROR != doSetRetransmitEndpoint(player)) ||
            // 调用IMediaPlayer（BpMediaPlayer）的同名方法
            (NO_ERROR != player->setDataSource(fd, offset, length))) {
            player.clear();
        }
        err = attachNewPlayer(player);
    }
    return err;
}
```

BpMediaPlayer最终会调用到BnMediaPlayer子类，也就是MediaPlayerService::Client的同名方法：

> 有兴趣可以看下MediaPlayer这块的binder接口定义，包括IMediaPlayerClient，IMediaPlayer，IMediaPlayerService。这里就不展开了。

> MediaPlayerService::Client

```c++
status_t MediaPlayerService::Client::setDataSource(int fd, int64_t offset, int64_t length)
{
	// 获取文件类型
    player_type playerType = MediaPlayerFactory::getPlayerType(this,
                                                               fd,
                                                               offset,
                                                               length);
    // 预处理，拿到MediaPlayerBase对象
    sp<MediaPlayerBase> p = setDataSource_pre(playerType);
    if (p == NULL) {
        return NO_INIT;
    }

    // 执行MediaPlayerBase的setDataSource
    return mStatus = setDataSource_post(p, p->setDataSource(fd, offset, length));
}
```

### 3.3.1 setDataSource_pre

不同的文件对应不同的播放器类，播放器类的公共父类为MediaPlayerBase，所以setDataSource_pre类似于一个生产MediaPlayerBase的工厂，传参是具体的player类型，下面看下这个工厂是如何实现的:

```C++
sp<MediaPlayerBase> MediaPlayerService::Client::setDataSource_pre(
        player_type playerType)
{

    sp<MediaPlayerBase> p = createPlayer(playerType);
    if (p == NULL) {
        return p;
    }

    if (!p->hardwareOutput()) {
        mAudioOutput = new AudioOutput(mAudioSessionId, IPCThreadState::self()->getCallingUid(),
                mPid, mAudioAttributes, mAudioDeviceUpdatedListener, mOpPackageName);
        static_cast<MediaPlayerInterface*>(p.get())->setAudioSink(mAudioOutput);
    }

    return p;
}
```

看下cratePlayer:

```C++
sp<MediaPlayerBase> MediaPlayerService::Client::createPlayer(player_type playerType)
{
    sp<MediaPlayerBase> p = getPlayer();
    if ((p != NULL) && (p->playerType() != playerType)) {
        ALOGV("delete player");
        p.clear();
    }
    if (p == NULL) {
        // getPlayer()返回mPlayer成员，此时必然为null，走到本分支
        p = MediaPlayerFactory::createPlayer(playerType, mListener, mPid);
    }

    if (p != NULL) {
        p->setUID(mUid);
    }

    return p;
}
```

ok，看下MediaPlayerFactory这个工厂类：

> MediaPlayerFactory

```C++
sp<MediaPlayerBase> MediaPlayerFactory::createPlayer(
        player_type playerType,
        const sp<MediaPlayerBase::Listener> &listener,
        pid_t pid) {
    sp<MediaPlayerBase> p;
    IFactory* factory;

    // 核心在于sFactoryMap是什么时候初始化的
    factory = sFactoryMap.valueFor(playerType);
    CHECK(NULL != factory);
    p = factory->createPlayer(pid);

    return p;
}
```

看下sFactoryMap的写入时机，找到一个函数：

```C++
status_t MediaPlayerFactory::registerFactory_l(IFactory* factory,
                                               player_type type) {

    if (sFactoryMap.add(type, factory) < 0) {
        ALOGE("Failed to register MediaPlayerFactory of type %d, failed to add"
              " to map.", type);
        return UNKNOWN_ERROR;
    }

    return OK;
}
```

搜索registerFactory_l的调用，找到两个核心的位置：

> 位置1：MediaPlayerFactory::registerFactory

```C++
status_t MediaPlayerFactory::registerFactory(IFactory* factory,
                                             player_type type) {
    Mutex::Autolock lock_(&sLock);
    return registerFactory_l(factory, type);
}
```

> 位置2：MediaPlayerFactory::registerBuiltinFacotories

```C++
void MediaPlayerFactory::registerBuiltinFactories() {
    Mutex::Autolock lock_(&sLock);

    if (sInitComplete)
        return;

    IFactory* factory = new NuPlayerFactory();
    if (registerFactory_l(factory, NU_PLAYER) != OK)
        delete factory;
    factory = new TestPlayerFactory();
    if (registerFactory_l(factory, TEST_PLAYER) != OK)
        delete factory;

    sInitComplete = true;
}
```

到这里可以大概知道至少有两个Player：

> NuPlayerFactory和TestPlayerFactory

后者明显是测试用的，实际上，手机厂商可以拓展registerBuiltinFactories()，从而实现自己的Player

> 那么问题来了，sFactoryMap中有那么多的Factory，MediaPlayerFactory是如何知道返回哪个Factory的呢

直接看下IFactory接口：

```C++
    class IFactory {
      public:
        virtual ~IFactory() { }

        virtual float scoreFactory(const sp<IMediaPlayer>& /*client*/,
                                   const char* /*url*/,
                                   float /*curScore*/) { return 0.0; }

        virtual float scoreFactory(const sp<IMediaPlayer>& /*client*/,
                                   int /*fd*/,
                                   int64_t /*offset*/,
                                   int64_t /*length*/,
                                   float /*curScore*/) { return 0.0; }

        virtual float scoreFactory(const sp<IMediaPlayer>& /*client*/,
                                   const sp<IStreamSource> &/*source*/,
                                   float /*curScore*/) { return 0.0; }

        virtual float scoreFactory(const sp<IMediaPlayer>& /*client*/,
                                   const sp<DataSource> &/*source*/,
                                   float /*curScore*/) { return 0.0; }

        virtual sp<MediaPlayerBase> createPlayer(pid_t pid) = 0;
    };
```

可以看到IFactory接口中定义了两个函数，一个是createPlayer()，也就是用来创建我们的MediaPlayerBase播放器的。另一个叫scoreFactory()，顾名思义，这个函数可以计算一个分数。

> 实际上，MediaPlayerFactory会根据媒体文件的类型，调用sFactoryMap中，每个Factory的scoreFactory()函数，返回得分最高的Factory。

这里返回的是NuPlayerFactory：

```C++
class NuPlayerFactory : public MediaPlayerFactory::IFactory {
  public:
    virtual float scoreFactory(const sp<IMediaPlayer>& /*client*/,
                               const char* url,
                               float curScore) {
        static const float kOurScore = 0.8;

        if (kOurScore <= curScore)
            return 0.0;

        if (!strncasecmp("http://", url, 7)
                || !strncasecmp("https://", url, 8)
                || !strncasecmp("file://", url, 7)) {
            size_t len = strlen(url);
            if (len >= 5 && !strcasecmp(".m3u8", &url[len - 5])) {
                return kOurScore;
            }

            if (strstr(url,"m3u8")) {
                return kOurScore;
            }

            if ((len >= 4 && !strcasecmp(".sdp", &url[len - 4])) || strstr(url, ".sdp?")) {
                return kOurScore;
            }
        }

        if (!strncasecmp("rtsp://", url, 7)) {
            return kOurScore;
        }

        return 0.0;
    }

    virtual float scoreFactory(const sp<IMediaPlayer>& /*client*/,
                               const sp<IStreamSource>& /*source*/,
                               float /*curScore*/) {
        return 1.0;
    }

    virtual float scoreFactory(const sp<IMediaPlayer>& /*client*/,
                               const sp<DataSource>& /*source*/,
                               float /*curScore*/) {
        // Only NuPlayer supports setting a DataSource source directly.
        return 1.0;
    }

    virtual sp<MediaPlayerBase> createPlayer(pid_t pid) {
        ALOGV(" create NuPlayer");
        return new NuPlayerDriver(pid);
    }
};
```

NuPlayerFactory创建的MediaPlayerBase是：NuPlayerDriver

### 3.3.2 setDataSource_post

继续看下setDataSource_post：

```C++
status_t MediaPlayerService::Client::setDataSource(int fd, int64_t offset, int64_t length)
{
	// 获取文件类型
    player_type playerType = MediaPlayerFactory::getPlayerType(this,
                                                               fd,
                                                               offset,
                                                               length);
    // 预处理，拿到MediaPlayerBase对象
    sp<MediaPlayerBase> p = setDataSource_pre(playerType);
    if (p == NULL) {
        return NO_INIT;
    }

    // 执行MediaPlayerBase的setDataSource
    return mStatus = setDataSource_post(p, p->setDataSource(fd, offset, length));
}
```

一步步来，首先是NuPlayerDriver::setDataSource()

> NuPlayerDriver::setDataSource

```C++
status_t NuPlayerDriver::setDataSource(int fd, int64_t offset, int64_t length) {
    ALOGV("setDataSource(%p) file(%d)", this, fd);
    Mutex::Autolock autoLock(mLock);

    if (mState != STATE_IDLE) {
        return INVALID_OPERATION;
    }

    mState = STATE_SET_DATASOURCE_PENDING;

    mPlayer->setDataSourceAsync(fd, offset, length);

    while (mState == STATE_SET_DATASOURCE_PENDING) {
        mCondition.wait(mLock);
    }

    return mAsyncResult;
}
```

OK，看来需要先搞清楚NuPlayerDriver::mPlayer是啥，啥时候初始化的：

```C++
// mPlayer是啥？
const sp<NuPlayer> mPlayer;

// mPlayer啥时候初始化的？
NuPlayerDriver::NuPlayerDriver(pid_t pid)
    : mState(STATE_IDLE),
      ...
      mPlayer(new NuPlayer(pid, mMediaClock)),
	  ...
```

好吧，猜测这个NuPlayerDriver也只是套壳，实际上执行功能的是NuPlayer

继续看NuPlayer::setDataSourceAsync()，顾名思义，这是个异步操作：

```c++
void NuPlayer::setDataSourceAsync(int fd, int64_t offset, int64_t length) {
    sp<AMessage> msg = new AMessage(kWhatSetDataSource, this);

    sp<AMessage> notify = new AMessage(kWhatSourceNotify, this);

    sp<GenericSource> source =
            new GenericSource(notify, mUIDValid, mUID, mMediaClock);

    status_t err = source->setDataSource(fd, offset, length);

    if (err != OK) {
        ALOGE("Failed to set data source!");
        source = NULL;
    }

    msg->setObject("source", source);
    msg->post();
    mDataSourceType = DATA_SOURCE_TYPE_GENERIC_FD;
}
```

emmm，看到了AMessage，应该是跟java侧的Handler类似的功能吧，姑且先理解为，NuPlayer把setDataSource的任务post到子线程去做了。具体关于AMessage，后面有时间再研究。

注意到有两个AMessage，首先看第一个kWhatSetDataSource，创建了一个GenericSource对象，作为消息内容发出：

```C++
void NuPlayer::onMessageReceived(const sp<AMessage> &msg) {
    switch (msg->what()) {
        case kWhatSetDataSource:
        {
            ALOGV("kWhatSetDataSource");

            CHECK(mSource == NULL);

            status_t err = OK;
            sp<RefBase> obj;
            CHECK(msg->findObject("source", &obj));
            if (obj != NULL) {
                Mutex::Autolock autoLock(mSourceLock);
                mSource = static_cast<Source *>(obj.get());
            } else {
                err = UNKNOWN_ERROR;
            }

            CHECK(mDriver != NULL);
            // 升级为强指针，然后通知NuPlayerDriver
            sp<NuPlayerDriver> driver = mDriver.promote();
            if (driver != NULL) {
                driver->notifySetDataSourceCompleted(err);
            }
            break;
        }
        ...
```

接收到kWhatSetDataSource的消息接收到后，直接通知下NuPlayerDriver，这块没有太多耗时的操作，异步与否其实无所谓。

重点看下另一个AMessage：kWhatSourceNotify

```C++
    sp<AMessage> notify = new AMessage(kWhatSourceNotify, this);

    sp<GenericSource> source =
            new GenericSource(notify, mUIDValid, mUID, mMediaClock);

    status_t err = source->setDataSource(fd, offset, length);
```

看下这个类：GenericSource：

> GenericSource

```C++
struct NuPlayer::GenericSource : public NuPlayer::Source,
                                 public MediaBufferObserver {
                                 }
```

GenericSource是NuPlayer::Source的一个子类，主要用于本地多媒体文件的读取解析，其他还有：

> NuPlayer::Source的子类有：GenericSource、HTTPLiveSource、RTSPSource、StreamingSource

先看GenericSource的构造函数：

```C++
NuPlayer::GenericSource::GenericSource(
        const sp<AMessage> &notify,
        bool uidValid,
        uid_t uid,
        const sp<MediaClock> &mediaClock)
    : Source(notify),
    	...  
	{
    ALOGV("GenericSource");
    CHECK(mediaClock != NULL);

    mBufferingSettings.mInitialMarkMs = kInitialMarkMs;
    mBufferingSettings.mResumePlaybackMarkMs = kResumePlaybackMarkMs;
    resetDataSource();
}

explicit Source(const sp<AMessage> &notify)
    : mNotify(notify) {
}

sp<AMessage> mNotify;
```

注意到notify被传过来了。

继续看GenericSource::setDataSource

```C++
status_t NuPlayer::GenericSource::setDataSource(
        int fd, int64_t offset, int64_t length) {
    Mutex::Autolock _l(mLock);
    ALOGV("setDataSource %d/%lld/%lld (%s)", fd, (long long)offset, (long long)length, nameForFd(fd).c_str());

    resetDataSource();

    mFd.reset(dup(fd));
    mOffset = offset;
    mLength = length;

    // delay data source creation to prepareAsync() to avoid blocking
    // the calling thread in setDataSource for any significant time.
    return OK;
}
```

做了写状态设置，跟AudioTrack没有什么关系，就不继续展开看了。

## 3.4 prepare

继续看prepare()函数：

> mediaplayer::prepare()

```C++
status_t MediaPlayer::prepare()
{
    ALOGV("prepare");
    Mutex::Autolock _l(mLock);
    mLockThreadId = getThreadId();
    if (mPrepareSync) {
        mLockThreadId = 0;
        return -EALREADY;
    }
    mPrepareSync = true;
    status_t ret = prepareAsync_l();
    if (ret != NO_ERROR) {
        mLockThreadId = 0;
        return ret;
    }

    if (mPrepareSync) {
        mSignal.wait(mLock);  // wait for prepare done
        mPrepareSync = false;
    }
    ALOGV("prepare complete - status=%d", mPrepareStatus);
    mLockThreadId = 0;
    return mPrepareStatus;
}
```

```c++
status_t MediaPlayer::prepareAsync_l()
{
    if ( (mPlayer != 0) && ( mCurrentState & (MEDIA_PLAYER_INITIALIZED | MEDIA_PLAYER_STOPPED) ) ) {
        if (mAudioAttributesParcel != NULL) {
            mPlayer->setParameter(KEY_PARAMETER_AUDIO_ATTRIBUTES, *mAudioAttributesParcel);
        } else {
            mPlayer->setAudioStreamType(mStreamType);
        }
        mCurrentState = MEDIA_PLAYER_PREPARING;
        return mPlayer->prepareAsync();
    }
    ALOGE("prepareAsync called in state %d, mPlayer(%p)", mCurrentState, mPlayer.get());
    return INVALID_OPERATION;
}
```

分析setDataSource的时候已知，MediaPlayer的mPlayer是MediaPlayerService::Client的代理：

> MediaPlayerService::Client

```C++
status_t MediaPlayerService::Client::prepareAsync()
{
    ALOGV("[%d] prepareAsync", mConnId);
    sp<MediaPlayerBase> p = getPlayer();
    if (p == 0) return UNKNOWN_ERROR;
    status_t ret = p->prepareAsync();
#if CALLBACK_ANTAGONIZER
    ALOGD("start Antagonizer");
    if (ret == NO_ERROR) mAntagonizer->start();
#endif
    return ret;
}
```

> NuPlayerDriver::prepareAsync

```C++
status_t NuPlayerDriver::prepareAsync() {
    ALOGV("prepareAsync(%p)", this);
    Mutex::Autolock autoLock(mLock);

    switch (mState) {
        case STATE_UNPREPARED:
            mState = STATE_PREPARING;
            mIsAsyncPrepare = true;
            mPlayer->prepareAsync();
            return OK;
        case STATE_STOPPED:
            // this is really just paused. handle as seek to start
            mAtEOS = false;
            mState = STATE_STOPPED_AND_PREPARING;
            mIsAsyncPrepare = true;
            mPlayer->seekToAsync(0, MediaPlayerSeekMode::SEEK_PREVIOUS_SYNC /* mode */,
                    true /* needNotify */);
            return OK;
        default:
            return INVALID_OPERATION;
    };
}
```

### 3.4.1 mState值是多少？

首先看下NuPlayerDriver::mState，在执行setDataSource后，NuPlayer会通知NuPlayerDriver执行完成：

```C++
void NuPlayer::onMessageReceived(const sp<AMessage> &msg) {
    switch (msg->what()) {
        case kWhatSetDataSource:
        {
            ALOGV("kWhatSetDataSource");

            CHECK(mSource == NULL);

            status_t err = OK;
            sp<RefBase> obj;
            CHECK(msg->findObject("source", &obj));
            if (obj != NULL) {
                Mutex::Autolock autoLock(mSourceLock);
                mSource = static_cast<Source *>(obj.get());
            } else {
                err = UNKNOWN_ERROR;
            }

            CHECK(mDriver != NULL);
            // 升级为强指针，然后通知NuPlayerDriver
            sp<NuPlayerDriver> driver = mDriver.promote();
            if (driver != NULL) {
                driver->notifySetDataSourceCompleted(err);
            }
            break;
        }
        ...
```

```C++
void NuPlayerDriver::notifySetDataSourceCompleted(status_t err) {
    Mutex::Autolock autoLock(mLock);

    CHECK_EQ(mState, STATE_SET_DATASOURCE_PENDING);

    mAsyncResult = err;
    mState = (err == OK) ? STATE_UNPREPARED : STATE_IDLE;
    mCondition.broadcast();
}
```

可以看到，此时mState为STATE_UNPREPARED

### 3.4.2 NuPlayer::prepareAsync

回到NuPlayerDriver::prepareAsync：

```C++
status_t NuPlayerDriver::prepareAsync() {
    ALOGV("prepareAsync(%p)", this);
    Mutex::Autolock autoLock(mLock);

    switch (mState) {
        case STATE_UNPREPARED:
            mState = STATE_PREPARING;
            mIsAsyncPrepare = true;
            mPlayer->prepareAsync();
            return OK;
        case STATE_STOPPED:
            // this is really just paused. handle as seek to start
            mAtEOS = false;
            mState = STATE_STOPPED_AND_PREPARING;
            mIsAsyncPrepare = true;
            mPlayer->seekToAsync(0, MediaPlayerSeekMode::SEEK_PREVIOUS_SYNC /* mode */,
                    true /* needNotify */);
            return OK;
        default:
            return INVALID_OPERATION;
    };
}
```

上面已经分析过了，mState为STATE_UNPREPARED，所以执行NuPlayer::prepareAsync()

```C++
void NuPlayer::prepareAsync() {
    ALOGV("prepareAsync");

    (new AMessage(kWhatPrepare, this))->post();
}
```

```C++
        case kWhatPrepare:
        {
            ALOGV("onMessageReceived kWhatPrepare");

            mSource->prepareAsync();
            break;
        }
```

对于本地播放源，mSource是GenericSource，继续跟踪：

```C++
void NuPlayer::GenericSource::prepareAsync() {
    Mutex::Autolock _l(mLock);
    ALOGV("prepareAsync: (looper: %d)", (mLooper != NULL));

    if (mLooper == NULL) {
        mLooper = new ALooper;
        mLooper->setName("generic");
        mLooper->start();

        mLooper->registerHandler(this);
    }

    sp<AMessage> msg = new AMessage(kWhatPrepareAsync, this);
    msg->post();
}
```

```C++
    switch (msg->what()) {
      case kWhatPrepareAsync:
      {
          onPrepareAsync();
          break;
      }
```

```c++
void NuPlayer::GenericSource::onPrepareAsync() {
    mDisconnectLock.lock();
    ALOGV("onPrepareAsync: mDataSource: %d", (mDataSource != NULL));

    // delayed data source creation
    if (mDataSource == NULL) {
        // set to false first, if the extractor
        // comes back as secure, set it to true then.
        mIsSecure = false;

        if (!mUri.empty()) {
            // 从网络创建MediaPlayer时才走此分支，此处省略
        } else {
            if (property_get_bool("media.stagefright.extractremote", true) &&
                    !PlayerServiceFileSource::requiresDrm(
                            mFd.get(), mOffset, mLength, nullptr /* mime */)) {
                sp<IBinder> binder =
                        defaultServiceManager()->getService(String16("media.extractor"));
                if (binder != nullptr) {
                    ALOGD("FileSource remote");
                    sp<IMediaExtractorService> mediaExService(
                            interface_cast<IMediaExtractorService>(binder));
                    sp<IDataSource> source;
                    mediaExService->makeIDataSource(base::unique_fd(dup(mFd.get())), mOffset, mLength, &source);
                    ALOGV("IDataSource(FileSource): %p %d %lld %lld",
                            source.get(), mFd.get(), (long long)mOffset, (long long)mLength);
                    if (source.get() != nullptr) {
                        mDataSource = CreateDataSourceFromIDataSource(source);
                    } else {
                        ALOGW("extractor service cannot make data source");
                    }
                } else {
                    ALOGW("extractor service not running");
                }
            }
            if (mDataSource == nullptr) {
                ALOGD("FileSource local");
                mDataSource = new PlayerServiceFileSource(dup(mFd.get()), mOffset, mLength);
            }
        }

        if (mDataSource == NULL) {
            ALOGE("Failed to create data source!");
            mDisconnectLock.unlock();
            notifyPreparedAndCleanup(UNKNOWN_ERROR);
            return;
        }
    }

    if (mDataSource->flags() & DataSource::kIsCachingDataSource) {
        mCachedSource = static_cast<NuCachedSource2 *>(mDataSource.get());
    }

    mDisconnectLock.unlock();

    // For cached streaming cases, we need to wait for enough
    // buffering before reporting prepared.
    mIsStreaming = (mCachedSource != NULL);

    // init extractor from data source
    status_t err = initFromDataSource();

    if (err != OK) {
        ALOGE("Failed to init from data source!");
        notifyPreparedAndCleanup(err);
        return;
    }

    if (mVideoTrack.mSource != NULL) {
        sp<MetaData> meta = getFormatMeta_l(false /* audio */);
        sp<AMessage> msg = new AMessage;
        err = convertMetaDataToMessage(meta, &msg);
        if(err != OK) {
            notifyPreparedAndCleanup(err);
            return;
        }
        notifyVideoSizeChanged(msg);
    }

    notifyFlagsChanged(
            // FLAG_SECURE will be known if/when prepareDrm is called by the app
            // FLAG_PROTECTED will be known if/when prepareDrm is called by the app
            FLAG_CAN_PAUSE |
            FLAG_CAN_SEEK_BACKWARD |
            FLAG_CAN_SEEK_FORWARD |
            FLAG_CAN_SEEK);

    finishPrepareAsync();

    ALOGV("onPrepareAsync: Done");
}
```

这块代码比较复杂，也不展开了，跟AudioTrack关系不大，后面有时间再研究下。

### 3.4.3 另一条分支。。。

再回到NuPlayerDriver::prepareAsync

```C++
status_t NuPlayerDriver::prepareAsync() {
    ALOGV("prepareAsync(%p)", this);
    Mutex::Autolock autoLock(mLock);

    switch (mState) {
        case STATE_UNPREPARED:
            mState = STATE_PREPARING;
            mIsAsyncPrepare = true;
            mPlayer->prepareAsync();
            return OK;
        case STATE_STOPPED:
            // this is really just paused. handle as seek to start
            mAtEOS = false;
            mState = STATE_STOPPED_AND_PREPARING;
            mIsAsyncPrepare = true;
            mPlayer->seekToAsync(0, MediaPlayerSeekMode::SEEK_PREVIOUS_SYNC /* mode */,
                    true /* needNotify */);
            return OK;
        default:
            return INVALID_OPERATION;
    };
}
```

当mState是STATE_STOPPED时，执行prepare()是会走到创建AudioTrack的流程的，具体流程见：

> https://blog.csdn.net/zhuyong006/article/details/86246540

这篇博客下面红框的部分可能是有误的，没有找到先prepare再stop再prepare的逻辑。

**只能暂时理解为，如果一个MediaPlayer执行过stop，再prepare的时候就会走到这个分支，直接创建AudioTrack。按这个理解，这个分支不是一个常见的场景，所以就不展开了，感兴趣可以参考上面的博客。**

综上，在没走到这个分支的情况下，调用prepare()是不会创建AudioTrack的

![image-20210921172130833](.\images\image-20210921172130833.png)

## 3.5 start

看下start，这次直奔主题，直接到NuPlayer::start

```C++
void NuPlayer::start() {
    (new AMessage(kWhatStart, this))->post();
}
```

```c++
        case kWhatStart:
        {
            ALOGV("kWhatStart");
            if (mStarted) {
                // do not resume yet if the source is still buffering
                if (!mPausedForBuffering) {
                    onResume();
                }
            } else {
                onStart();
            }
            mPausedByClient = false;
            break;
        }
```

来到NuPlayer::onStart()

```C++
void NuPlayer::onStart(int64_t startPositionUs, MediaPlayerSeekMode mode) {
    // 上面的代码都忽略，只看最后一行
    ...

    postScanSources();
}
```

```C++
void NuPlayer::postScanSources() {
    if (mScanSourcesPending) {
        return;
    }

    sp<AMessage> msg = new AMessage(kWhatScanSources, this);
    msg->setInt32("generation", mScanSourcesGeneration);
    msg->post();

    mScanSourcesPending = true;
}
```

```C++
case kWhatScanSources:
        {
            // 仅保留跟AudioTrack创建有关的
            if (mAudioSink != NULL && mAudioDecoder == NULL) {
                if (instantiateDecoder(true, &mAudioDecoder) == -EWOULDBLOCK) {
                    rescan = true;
                }
            }
        }
```

继续，来到instantiateDecoder()

```C++
status_t NuPlayer::instantiateDecoder(
        bool audio, sp<DecoderBase> *decoder, bool checkAudioModeChange) {
    if (audio) {
        sp<AMessage> notify = new AMessage(kWhatAudioNotify, this);
        ++mAudioDecoderGeneration;
        notify->setInt32("generation", mAudioDecoderGeneration);

        if (checkAudioModeChange) {
            determineAudioModeChange(format);
        }
    }
}
```

```C++
void NuPlayer::determineAudioModeChange(const sp<AMessage> &audioFormat) {
    if (canOffload) {
        if (!mOffloadAudio) {
            mRenderer->signalEnableOffloadAudio();
        }
        // open audio sink early under offload mode.
        tryOpenAudioSinkForOffload(audioFormat, audioMeta, hasVideo);
    } else {
        if (mOffloadAudio) {
            mRenderer->signalDisableOffloadAudio();
            mOffloadAudio = false;
        }
    }
}
```

```C++
void NuPlayer::tryOpenAudioSinkForOffload(
        const sp<AMessage> &format, const sp<MetaData> &audioMeta, bool hasVideo) {

    status_t err = mRenderer->openAudioSink(
            format, true /* offloadOnly */, hasVideo,
            AUDIO_OUTPUT_FLAG_NONE, &mOffloadAudio, mSource->isStreaming());
    if (err != OK) {
        // Any failure we turn off mOffloadAudio.
        mOffloadAudio = false;
    } else if (mOffloadAudio) {
        sendMetaDataToHal(mAudioSink, audioMeta);
    }
}
```

mRenderer对应NuPlayerRenderer：

```c++
status_t NuPlayer::Renderer::openAudioSink(
        const sp<AMessage> &format,
        bool offloadOnly,
        bool hasVideo,
        uint32_t flags,
        bool *isOffloaded,
        bool isStreaming) {
    sp<AMessage> msg = new AMessage(kWhatOpenAudioSink, this);
    msg->setMessage("format", format);
    msg->setInt32("offload-only", offloadOnly);
    msg->setInt32("has-video", hasVideo);
    msg->setInt32("flags", flags);
    msg->setInt32("isStreaming", isStreaming);

    sp<AMessage> response;
    status_t postStatus = msg->postAndAwaitResponse(&response);

    int32_t err;
    if (postStatus != OK || response.get() == nullptr || !response->findInt32("err", &err)) {
        err = INVALID_OPERATION;
    } else if (err == OK && isOffloaded != NULL) {
        int32_t offload;
        CHECK(response->findInt32("offload", &offload));
        *isOffloaded = (offload != 0);
    }
    return err;
}
```

kWhatOpenAudioSink处理线程：

```C++
        case kWhatOpenAudioSink:
        {
            sp<AMessage> format;
            CHECK(msg->findMessage("format", &format));

            int32_t offloadOnly;
            CHECK(msg->findInt32("offload-only", &offloadOnly));

            int32_t hasVideo;
            CHECK(msg->findInt32("has-video", &hasVideo));

            uint32_t flags;
            CHECK(msg->findInt32("flags", (int32_t *)&flags));

            uint32_t isStreaming;
            CHECK(msg->findInt32("isStreaming", (int32_t *)&isStreaming));

            status_t err = onOpenAudioSink(format, offloadOnly, hasVideo, flags, isStreaming);

            sp<AMessage> response = new AMessage;
            response->setInt32("err", err);
            response->setInt32("offload", offloadingAudio());

            sp<AReplyToken> replyID;
            CHECK(msg->senderAwaitsResponse(&replyID));
            response->postReply(replyID);

            break;
        }
```

onOpenAudioSink：

```C++
status_t NuPlayer::Renderer::onOpenAudioSink(
        const sp<AMessage> &format,
        bool offloadOnly,
        bool hasVideo,
        uint32_t flags,
        bool isStreaming) {

            err = mAudioSink->open(
                    sampleRate,
                    numChannels,
                    (audio_channel_mask_t)channelMask,
                    audioFormat,
                    0 /* bufferCount - unused */,
                    &NuPlayer::Renderer::AudioSinkCallback,
                    this,
                    (audio_output_flags_t)offloadFlags,
                    &offloadInfo);


}
```

这里的mAudioSink中的setDataSource_pre有完成设置，指向的是MediaPlayerService中的AudioOutput

```C++
status_t MediaPlayerService::AudioOutput::open(
        uint32_t sampleRate, int channelCount, audio_channel_mask_t channelMask,
        audio_format_t format, int bufferCount,
        AudioCallback cb, void *cookie,
        audio_output_flags_t flags,
        const audio_offload_info_t *offloadInfo,
        bool doNotReconnect,
        uint32_t suggestedFrameCount)
{
   
            t = new AudioTrack(
                    mStreamType,
                    sampleRate,
                    format,
                    channelMask,
                    frameCount,
                    flags,
                    NULL, // callback
                    NULL, // user data
                    0, // notification frames
                    mSessionId,
                    AudioTrack::TRANSFER_DEFAULT,
                    NULL, // offload info
                    mUid,
                    mPid,
                    mAttributes,
                    doNotReconnect,
                    targetSpeed,
                    mSelectedDeviceId,
                    mOpPackageName);

			mTrack = t;
}
```

综上，总算是找到了AudioTrack的创建位置，最终赋值给了MediaPlayerService的mTrack成员

# 4. AudioTrack(JAVA API)

Android面向应用开发者提供了MediaPlayer，AudioTrack，SoundPool等Java Api、提供AAudio等NDK接口用于播放声音。MediaPlayer可以播放MP3/FLAC/OGG等各种格式的声音，而AudioTrack只能播放解码后的PCM数据流。通过上一节“MediaPlayer到AudioTrack”，我们知道MediaPlayer也是会创建AudioTrack的。事实上，MediaPlayer内部会通过解码器先将音频输入解码，解码后通过AudioTrack播放。

本节主要介绍下AudioTrack的java api，并以此为契机说明下相关的几个音频概念。

## 4.1 构造方法

直接看AudioTrack.java的构造方法：

```JAVA
AudioTrack(int streamType, int sampleRateInHz, int channelConfig, int audioFormat, int bufferSizeInBytes, 
           int mode, int sessionId)
```

这个构造方法当前已经废弃了，google推荐使用AudioTrack::Builder()结合具备更丰富功能的AudioAttribute进行AudioTrack的初始化。

但方便起见，这里我们还使用这个构造方法。

下面，基于这个构造方法，逐个说明这些参数：

### 4.1.1 流类型

Android定义了很多流类型，以进行灵活的音频管理策略。例如，调节媒体音（STREAM_MUSIC）大小，不会影响到通话音（STREAM_VOICE_CALL）;再如，手机连接这蓝牙音箱的时候，我只把媒体音投放到蓝牙音箱，让家人们听音乐，而通话音仍保留在手机上，可以做到通话音乐两不误。

Android中的流类型（Stream type）定义在AudioManager.java中：

| 名称                              | 值                                   | 含义                       |
| --------------------------------- | ------------------------------------ | -------------------------- |
| AudioManager.STREAM_VOICE_CALL    | AudioSystem.STREAM_VOICE_CALL = 0    | 通话音                     |
| AudioManager.STREAM_SYSTEM        | AudioSystem.STREAM_SYSTEM = 1        | 系统音                     |
| AudioManager.STREAM_RING          | AudioSystem.STREAM_RING = 2          | 响铃音                     |
| AudioManager.STREAM_MUSIC         | AudioSystem.STREAM_MUSIC = 3         | 媒体音                     |
| AudioManager.STREAM_ALARM         | AudioSystem.STREAM_ALARM = 4         | 闹铃音                     |
| AudioManager.STREAM_NOTIFICATION  | AudioSystem.STREAM_NOTIFICATION = 5  | 通知音                     |
| AudioManager.STREAM_BLUETOOTH_SCO | AudioSystem.STREAM_BLUETOOTH_SCO = 6 | 蓝牙sco                    |
| AudioManager.STREAM_ENFORCED      | AudioSystem.STREAM_ENFORCED = 7      | 强制音（例如日本的拍照音） |
| AudioManager.STREAM_DTMF          | AudioSystem.STREAM_DTMF = 8          | 拨号盘按键音               |

虽然有很多流类型，但是很多是共享一个音量的。

### 4.1.2 采样率

> 采样：连续的时间信号进行周期性的扫描，变成时间上离散的瞬时信号的过程，采样遵循采样定理
>
> 采样率：每秒采样次数
>
> 采样定理（又名奈奎斯特定理）：当采样频率fs大于信号中最高频率fmax的2倍时(fs>2fmax)，采样之后的数字信号完整地保留了原始信号中的信息，也就是说可以不失真的恢复出原始的模拟信号。

人耳能听到的频率范围是20~20kHz，根据采样定理，fs>40kHz的时候就可以完整记录20kHz以下的音频信号，就可以完整地保存人耳能听到的音频数据。

常用的采样频率：

8kHz、11.025kHz、22.05kHz、16kHz、37.8kHz、44.1kHz、48kHz、96kHz、192kHz

AudioFormat.java中，定义了几个跟采样率有关的常量：

| 名称                           | 值     | 含义       |
| ------------------------------ | ------ | ---------- |
| AudioFormat.SAMPLE_RATE_HZ_MIN | 4000   | 最低采样率 |
| AudioFormat.SAMPLE_RATE_HZ_MAX | 192000 | 最高采样率 |

### 4.1.3 声道

> 声道(Sound Channel) 是指声音在录制或播放时在不同空间位置采集或回放的相互独立的[音频信号](https://baike.baidu.com/item/音频信号/3431469)，所以声道数也就是声音录制时的音源数量或回放时相应的扬声器数量。

声道类型包含单声道、立体声、5.1声道、7.1声道

Android中的声道数（Sound Channel）定义在AudioFormat.java中：

| 名称                           | 值                                                 | 含义   |
| ------------------------------ | -------------------------------------------------- | ------ |
| AudioFormat.CHANNEL_OUT_MONO   | AudioFormat.CHANNEL_OUT_FRONT_LEFT = 0x4           | 单声道 |
| AudioFormat.CHANNEL_OUT_STEREO | (CHANNEL_OUT_FRONT_LEFT \|CHANNEL_OUT_FRONT_RIGHT) | 立体声 |
| 其他（略）                     |                                                    |        |

注意到AudioFormat中还定义了CHANNEL_IN_MONO、CHANNEL_IN_STEREO等，这些用于录音场景（AudioRecorder）。

### 4.1.4 位宽

> 位宽（Audio Format）又称采样位数、采样精度，代表采样数据的精确程度。位宽越大，采样数据的解析度/分辨率越高，声音也更自然。
>
> 例如，8位代表音乐信息有2的8次方，也就是256个精度单位。16位有64k个精度单位，前者会造成精度丢失。

Android中的位宽（Audio Format）定义在AudioFormat.java中：

| 名称                           | 值   | 含义          |
| ------------------------------ | ---- | ------------- |
| AudioFormat.ENCODING_INVALID   | 0    | 非法位宽      |
| AudioFormat.ENCODING_DEFAULT   | 1    | 默认位宽      |
| AudioFormat.ENCODING_PCM_16BIT | 2    | 16位/每采样点 |
| AudioFormat.ENCODING_PCM_32BIT | 3    | 32位/每采样点 |

### 4.1.5 BufferSizeInBytes

> BufferSizeInBytes是最重要的一个参数，它配置的是AudioTrack内部音频缓冲区的大小，它是声音能正常播放的最低保障。
>
> BufferSizeInBytes跟AudioTrack的mode也有关。
>
> 当处于MODE_STATIC模式时，BufferSizeInBytes是待播放的采样数据的总大小
>
> 当处于MODE_STREAM模式，代表缓冲区最小大小，使用getMinBufferSize()计算

AudioTrack提供了一个帮开发者确认最低内部缓冲区大小的方法，原型如下：

```java
int getMinBufferSize(int sampleRateInHz, int channelConfig, int audioFormat) {
	return native_get_min_buff_size(sampleRateInHz, channelCount, audioFormat);
}
```

从传参可以看到，最小音频缓冲区大小，是跟上面提到的采样率、声道和位宽相关的。

继续查看JNI层：

```C++
// 查看JNI
static jint android_media_AudioTrack_get_min_buff_size(JNIEnv *env,  jobject thiz,
    jint sampleRateInHertz, jint channelCount, jint audioFormat) {

    size_t frameCount;
    const status_t status = AudioTrack::getMinFrameCount(&frameCount, AUDIO_STREAM_DEFAULT,
            sampleRateInHertz);
    if (status != NO_ERROR) {
        ALOGE("AudioTrack::getMinFrameCount() for sample rate %d failed with status %d",
                sampleRateInHertz, status);
        return -1;
    }
    const audio_format_t format = audioFormatToNative(audioFormat);
    if (audio_has_proportional_frames(format)) {
        const size_t bytesPerSample = audio_bytes_per_sample(format);
        // frameCount：通过采样率计算得到的最低音频帧数
        // channelCount：声道
        // bytesPerSample：通过位宽计算得到的每帧字节数
        return frameCount * channelCount * bytesPerSample;
    } else {
        return frameCount;
    }
}
```

$$
最小缓冲区大小
	= 最小采样时间 * 采样率 * 声道数 * 位宽
    = 最小帧数 * 声道数 * 每帧字节数
$$

### 4.1.6 mode

AudioTrack有两种数据传输模式，定义在AudioTrack.java

| MODE                   | 含义                                                         |
| ---------------------- | ------------------------------------------------------------ |
| AudioTrack.MODE_STATIC | 应用进程将回放数据一次性付给 AudioTrack，适用于数据量小、时延要求高的场景 |
| AudioTrack.MODE_STREAM | 用进程需要持续调用 write() 写数据到 FIFO，写数据时有可能遭遇阻塞（等待 AudioFlinger::PlaybackThread 消费之前的数据），基本适用所有的音频场景 |



两种mode下创建AudioTrack对象的区别：

> MODE_STATIC下，bufferSizeInBytes就是待播放文件的大小：

```java
//file 就是需要播放的音频文件，这里的buffersize就是文件的大小
audioTrack = new AudioTrack(AudioManager.STREAM_MUSIC,
        44100, AudioFormat.CHANNEL_OUT_STEREO,
        AudioFormat.ENCODING_PCM_16BIT, (int) file.length(), AudioTrack.MODE_STATIC);
```

> MODE_STREAM下，bufferSizeInBytes需要通过getMinBufferSize()计算

```java
 bufferSize = AudioTrack.getMinBufferSize(44100, AudioFormat.CHANNEL_OUT_STEREO, AudioFormat.ENCODING_PCM_16BIT);
        audioTrack = new AudioTrack(AudioManager.STREAM_MUSIC,
                44100, AudioFormat.CHANNEL_OUT_STEREO, AudioFormat.ENCODING_PCM_16BIT, bufferSize, AudioTrack.MODE_STREAM);
```

## 4.2 关键成员

### 4.2.1 mState

一般结合getState()方法，用于在AudioTrack构造方法执行后，判断是否初始化成功

```java
        mAudioTrack = new AudioTrack(streamType,sampleRateInHz,channelConfig,audioFormat,mMinBufferSize,DEFAULT_PLAY_MODE);
        if (mAudioTrack.getState() == AudioTrack.STATE_UNINITIALIZED) {
            Log.e(TAG, "AudioTrack initialize fail !");
            return false;
        }   
```

mState有三个状态：

| 名称                             | 值   | 含义                                                         |
| -------------------------------- | ---- | ------------------------------------------------------------ |
| AudioFormat.STATE_UNINITIALIZED  | 0    | AudioTrack未成功初始化                                       |
| AudioFormat.STATE_INITIALIZED    | 1    | AudioTrack成功初始化                                         |
| AudioFormat.STATE_NO_STATIC_DATA | 2    | 当前是使用 MODE_STATIC ，但是还没往缓冲区中写入数据。当接收数据之后会变为 STATE_INITIALIZED 状态 |

![image-20211002111338214](.\images\image-20211002111338214.png)

### 4.2.2 mPlayState

播放状态

| 名称                          | 值   | 含义 |
| ----------------------------- | ---- | ---- |
| AudioFormat.PLAYSTATE_STOPPED | 1    | 停止 |
| AudioFormat.PLAYSTATE_PAUSED  | 2    | 暂停 |
| AudioFormat.PLAYSTATE_PLAYING | 3    | 播放 |

### 4.2.3 mDataLoadMode

保存mode，有两个值：MODE_STATIC，MODE_STREAM

## 4.3 关键方法

### 4.3.1 写数据（write）

```JAVA
int write(@NonNull byte[] audioData, int offsetInBytes, int sizeInBytes,
            @WriteMode int writeMode)
```

参数含义：

| 名称          | 含义               |
| ------------- | ------------------ |
| audioData     | 待写入数据         |
| offsetInBytes | 待写入数据起始位置 |
| sizeInBytes   | 待写入数据长度     |
| writeMode     | 写入模式           |

MODE_STATIC模式下，调用write()之后，AudioTrack的mState就会从STATE_NO_STATIC_DATA切换到STATE_INITIALIZED，紧接着就可以调用play()播放了。此外，MODE_STATIC模式下，AudioTrack是直接进行memcopy的，耗时短，所以设计为同步方法。

MODE_STREAM模式下，write()可能会阻塞，需要在子线程中执行write()。

> 详细内容会在AudioTrack（native层）详细说明，本节只说明AudioTrack java层API的使用方法。

### 4.3.2 播放（play）

MODE_STATIC模式下，write()完毕可以直接play()进行播放

```java
//先将所有的数据写入到缓冲区
write(...)
//然后在播放
play(..)
```

MODE_STREAM模式下，先调用play()，然后子线程中不断write()写入数据

```java
paly(...)

new Thread() {
    public void run() {
        //一系列的 write 操作
        `write(...)`
    }
    
}.start();
```

### 4.3.3 停止播放（stop）

> 对于 `MODE_STREAM` 模式，如果单是调用 stop 方法， AudioTrack 会等待缓冲的最后一帧数据播放完毕之后，才会停止，如果需要立即停止，那么就需要调用 `pause` 然后调用 `flush` 这两个方法，那么 AudioTrack 就是丢缓冲区中剩余的数据。

```java
void stop ()
```

### 4.3.4 暂停播放（pause）

> 暂停播放，但是缓冲区中没有被播放的数据不会被舍弃，调用 play 方法即可接着播放

```java
void pause ()
```

### 4.3.5 刷新（flush）

> 刷新正在排队播放的音频数据，调用该方法会将写入到缓冲区但没有被播放的音频数据都会被丢弃。如果是非 STREAM 或者没有执行 pasuse 或者 stop 将不会有任何效果。

```java
void flush()
```

### 4.3.6 释放（release）

> 释放本地 AudioTrack 对象。

```java
void release ()
```

## 4.4 代码示例

### 4.4.1 静态播放示例

```java
// ************ 静态播放模式 ************ 
// 直接获取文件大小
InputStream in = getResources().openRawResource(R.raw.ding);
try {
   try {
       ByteArrayOutputStream out = new ByteArrayOutputStream();
       for (int b; (b = in.read()) != -1; ) {
           out.write(b);
       }
       audioData = out.toByteArray();
   } finally {
       in.close();
   }
} catch (IOException ioe) {
   ioe.printStackTrace();
   Log.d(TAG, "读取数据失败！");
}
//构造AudioTrack对象，写入数据并播放
audioTrack = new AudioTrack(
new AudioAttributes.Builder()
        .setUsage(AudioAttributes.USAGE_MEDIA)
        .setContentType(AudioAttributes.CONTENT_TYPE_MUSIC)
        .build(),
new AudioFormat.Builder().setSampleRate(22050)
        .setEncoding(AudioFormat.ENCODING_PCM_8BIT)
        .setChannelMask(AudioFormat.CHANNEL_OUT_MONO)
        .build(),
audioData.length,
AudioTrack.MODE_STATIC,
AudioManager.AUDIO_SESSION_ID_GENERATE);
audioTrack.write(audioData, 0, audioData.length);
if(audioTrack.getState() == AudioTrack.STATE_UNINITIALIZED){
    Toast.makeText(this,"AudioTrack初始化失败！",Toast.LENGTH_SHORT).show();
    return;
} 
audioTrack.play();
```

### 4.4.2 流播放示例

```java
// ************ 流播放 ************ 
final int minBufferSize = AudioTrack.getMinBufferSize(SAMPLE_RATE_INHZ, AudioFormat.CHANNEL_OUT_MONO, AUDIO_FORMAT);
// 创建 AudioTrack 对象
audioTrack = new AudioTrack(
 new AudioAttributes.Builder()
         .setUsage(AudioAttributes.USAGE_MEDIA)
         .setContentType(AudioAttributes.CONTENT_TYPE_MUSIC)
         .build(),
 new AudioFormat.Builder().setSampleRate(SAMPLE_RATE_INHZ)
         .setEncoding(AUDIO_FORMAT)
         .setChannelMask(AudioFormat.CHANNEL_OUT_MONO)
         .build(),
 minBufferSize,
 AudioTrack.MODE_STREAM,
 AudioManager.AUDIO_SESSION_ID_GENERATE
);
// 检查初始化是否成功
if(audioTrack.getState() == AudioTrack.STATE_UNINITIALIZED){
    Toast.makeText(this,"AudioTrack初始化失败！",Toast.LENGTH_SHORT).show();
    return;
} 
// 播放
audioTrack.play();
//子线程中文件流写入
workHandler.post(new Runnable() {
   @Override
   public void run() {
       try {
           final File file = new File(getExternalFilesDir(Environment.DIRECTORY_MUSIC), "test.pcm");
           FileInputStream fileInputStream = new FileInputStream(file);
           byte[] tempBuffer = new byte[minBufferSize];
           while (fileInputStream.available() > 0) {
               int readCount = fileInputStream.read(tempBuffer);
               if (readCount == AudioTrack.ERROR_INVALID_OPERATION ||
                       readCount == AudioTrack.ERROR_BAD_VALUE) {
                   continue;
               }
               if (readCount != 0 && readCount != -1) {
                   audioTrack.write(tempBuffer, 0, readCount);
               }
           }
           fileInputStream.close();
       } catch (IOException ioe) {
           ioe.printStackTrace();
       }
   }
});
```

### 4.4.3 停止播放

```java
// 停止线程
handlerThread.quit();
workHandler.removeCallbacksAndMessages(null);
if(audioTrack.getState() != AudioTrack.STATE_UNINITIALIZED){
    audioTrack.stop();
    audioTrack.release();
}
```

# 5. AudioTrack(native API)

native层的接口位于AudioTrack.cpp，native层接口所处的位置可以用下图说明：

![image-20211003203046859](.\images\image-20211003203046859.png)



涉及到走AudioTrack进行音频播放的，包括MediaPlayer.java、AudioTrack.java，都需要通过AudioTrack.cpp走binder与audioserver进程进行通信，进而实现播放过程。

## 5.1 构造函数

> AudioTrack.cpp

```C++
AudioTrack::AudioTrack(
        audio_stream_type_t streamType,			// 流类型
        uint32_t sampleRate,					// 采样率
        audio_format_t format,					// 位宽
        audio_channel_mask_t channelMask,		// 声道数
        const sp<IMemory>& sharedBuffer,		// 共享内存缓冲区：数据模式是 MODE_STATIC 时使用，数据模式是 MODE_STREAM 时为空
        audio_output_flags_t flags,				// 音频输出flag
        callback_t cbf,							// 回调
        void* user,								// TODO
        int32_t notificationFrames,				
        audio_session_t sessionId,				// SessionId
        transfer_type transferType,				// 数据如何传递给AudioTrack
        const audio_offload_info_t *offloadInfo,
        uid_t uid,
        pid_t pid,
        const audio_attributes_t* pAttributes,
        bool doNotReconnect,					// 缺省为false，代表自动重连
        float maxRequiredSpeed,
        const std::string& opPackageName)
    : mStatus(NO_INIT),
      mState(STATE_STOPPED),
      mPreviousPriority(ANDROID_PRIORITY_NORMAL),
      mPreviousSchedulingGroup(SP_DEFAULT),
      mPausedPosition(0),
      mSelectedDeviceId(AUDIO_PORT_HANDLE_NONE),
      mOpPackageName(opPackageName),
      mAudioTrackCallback(new AudioTrackCallback())
{
    mAttributes = AUDIO_ATTRIBUTES_INITIALIZER;

    (void)set(streamType, sampleRate, format, channelMask,
            0 /*frameCount*/, flags, cbf, user, notificationFrames,
            sharedBuffer, false /*threadCanCallJava*/, sessionId, transferType, offloadInfo,
            uid, pid, pAttributes, doNotReconnect, maxRequiredSpeed);
}
```

入参比较多：

| 入参类型             | 参数名       | 类型定义位置                                                 | 类型定义so                 | 含义           |
| -------------------- | ------------ | ------------------------------------------------------------ | -------------------------- | -------------- |
| audio_stream_type_t  | streamType   | system/media/audio/<br>include/system/<br>audio-hal-enums.h  | libaudio_system_headers.so | 流类型         |
| uint32_t             | sampleRate   |                                                              |                            | 采样率         |
| audio_format_t       | format       | system/media/audio/<br>include/system/<br>audio-hal-enums.h  | libaudio_system_headers.so | 位宽           |
| audio_channel_mask_t | channelMask  | system/media/audio/<br>include/system/<br>audio-hal-enums.h  | libaudio_system_headers.so | 声道数         |
| sp<IMemory>&         | sharedBuffer |                                                              |                            | 共享内存缓冲区 |
| audio_output_flags_t |              | system/media/audio/<br/>include/system/<br/>audio-hal-enums.h | libaudio_system_headers.so |                |
|                      |              |                                                              |                            |                |
|                      |              |                                                              |                            |                |
|                      |              |                                                              |                            |                |

逐个展开看下：

### 5.1.1 audio_stream_type_t（流类型）

> system/media/audio/include/system/audio-hal-enums.h

```C++
typedef enum {
    AUDIO_STREAM_LIST_DEF(AUDIO_DEFINE_ENUM_SYMBOL_V)
} audio_stream_type_t;
```

继续看下AUDIO_STREAM_LIST_DEF的定义：

```c++
#define AUDIO_STREAM_LIST_DEF(V)
    AUDIO_STREAM_LIST_NO_SYS_DEF(V)
    V(AUDIO_STREAM_DEFAULT, -1)
```

把这段替换，得到：

```c++
typedef enum {
	AUDIO_STREAM_LIST_NO_SYS_DEF(AUDIO_DEFINE_ENUM_SYMBOL_V)
	AUDIO_DEFINE_ENUM_SYMBOL_V(AUDIO_STREAM_DEFAULT, -1)
} audio_stream_type_t;
```

继续，看下AUDIO_STREAM_LIST_NO_SYS_DEF的定义

```c++
#define AUDIO_STREAM_LIST_NO_SYS_DEF(V)
    V(AUDIO_STREAM_VOICE_CALL, 0) \
    V(AUDIO_STREAM_SYSTEM, 1) \
    V(AUDIO_STREAM_RING, 2) \
    V(AUDIO_STREAM_MUSIC, 3) 
    V(AUDIO_STREAM_ALARM, 4) 
    V(AUDIO_STREAM_NOTIFICATION, 5) 
    V(AUDIO_STREAM_BLUETOOTH_SCO, 6) 
    V(AUDIO_STREAM_ENFORCED_AUDIBLE, 7) 
    V(AUDIO_STREAM_DTMF, 8) 
    V(AUDIO_STREAM_TTS, 9) 
    V(AUDIO_STREAM_ACCESSIBILITY, 10) 
    V(AUDIO_STREAM_ASSISTANT, 11) 
    V(AUDIO_STREAM_REROUTING, 12) 
    V(AUDIO_STREAM_PATCH, 13) 
    V(AUDIO_STREAM_CALL_ASSISTANT, 14)
```

把这段再替换下，得到

```c++
typedef enum {
    AUDIO_DEFINE_ENUM_SYMBOL_V(AUDIO_STREAM_VOICE_CALL, 0)
    AUDIO_DEFINE_ENUM_SYMBOL_V(AUDIO_STREAM_SYSTEM, 1) 
    AUDIO_DEFINE_ENUM_SYMBOL_V(AUDIO_STREAM_RING, 2) 
    AUDIO_DEFINE_ENUM_SYMBOL_V(AUDIO_STREAM_MUSIC, 3) 
    AUDIO_DEFINE_ENUM_SYMBOL_V(AUDIO_STREAM_ALARM, 4) 
    AUDIO_DEFINE_ENUM_SYMBOL_V(AUDIO_STREAM_NOTIFICATION, 5) 
    AUDIO_DEFINE_ENUM_SYMBOL_V(AUDIO_STREAM_BLUETOOTH_SCO, 6) 
    AUDIO_DEFINE_ENUM_SYMBOL_V(AUDIO_STREAM_ENFORCED_AUDIBLE, 7) 
    AUDIO_DEFINE_ENUM_SYMBOL_V(AUDIO_STREAM_DTMF, 8) 
    AUDIO_DEFINE_ENUM_SYMBOL_V(AUDIO_STREAM_TTS, 9) 
    AUDIO_DEFINE_ENUM_SYMBOL_V(AUDIO_STREAM_ACCESSIBILITY, 10) 
    AUDIO_DEFINE_ENUM_SYMBOL_V(AUDIO_STREAM_ASSISTANT, 11) 
    AUDIO_DEFINE_ENUM_SYMBOL_V(AUDIO_STREAM_REROUTING, 12) 
    AUDIO_DEFINE_ENUM_SYMBOL_V(AUDIO_STREAM_PATCH, 13) 
    AUDIO_DEFINE_ENUM_SYMBOL_V(AUDIO_STREAM_CALL_ASSISTANT, 14)
	AUDIO_DEFINE_ENUM_SYMBOL_V(AUDIO_STREAM_DEFAULT, -1)
} audio_stream_type_t;
```

继续，看下AUDIO_DEFINE_ENUM_SYMBOL_V的定义：

```c++
#define AUDIO_DEFINE_ENUM_SYMBOL_V(symbol, value) symbol = value
```

再替换下，最终得到：

```c++
typedef enum {
    AUDIO_STREAM_VOICE_CALL = 0
    AUDIO_STREAM_SYSTEM = 1 
    AUDIO_STREAM_RING = 2 
    AUDIO_STREAM_MUSIC = 3 
    AUDIO_STREAM_ALARM = 4
    AUDIO_STREAM_NOTIFICATION = 5 
    AUDIO_STREAM_BLUETOOTH_SCO = 6
    AUDIO_STREAM_ENFORCED_AUDIBLE = 7
    AUDIO_STREAM_DTMF = 8 
    AUDIO_STREAM_TTS = 9
    AUDIO_STREAM_ACCESSIBILITY = 10 
    AUDIO_STREAM_ASSISTANT = 11 
    AUDIO_STREAM_REROUTING = 12
    AUDIO_STREAM_PATCH = 13
    AUDIO_STREAM_CALL_ASSISTANT = 14
	AUDIO_STREAM_DEFAULT = -1			// 
} audio_stream_type_t;
```

终于“翻译”成比较好理解的写法了，不知道C++为何要设计那么多#define

流类型的定义与java层基本一致，就不细看了。

### 5.1.2 audio_format_t（位宽）

audio_format_t同样定义在libaudio_system_headers.so，直接看替换各种#define后的结果：

```c++
typedef enum {
    AUDIO_FORMAT_DEFAULT = AUDIO_FORMAT_PCM_MAIN
    AUDIO_FORMAT_PCM_16_BIT = AUDIO_FORMAT_PCM_MAIN | AUDIO_FORMAT_PCM_SUB_16_BIT
    AUDIO_FORMAT_PCM_8_BIT = AUDIO_FORMAT_PCM_MAIN | AUDIO_FORMAT_PCM_SUB_8_BIT
    AUDIO_FORMAT_PCM_32_BIT = AUDIO_FORMAT_PCM_MAIN | AUDIO_FORMAT_PCM_SUB_32_BIT
    AUDIO_FORMAT_PCM_8_24_BIT = AUDIO_FORMAT_PCM_MAIN | AUDIO_FORMAT_PCM_SUB_8_24_BIT
    AUDIO_FORMAT_PCM_FLOAT = AUDIO_FORMAT_PCM_MAIN | AUDIO_FORMAT_PCM_SUB_FLOAT
    ......
    AUDIO_FORMAT_INVALID = 0xFFFFFFFFu,
    AUDIO_FORMAT_PCM = AUDIO_FORMAT_PCM_MAIN,
} audio_format_t
```

### 5.1.3 audio_channel_mask_t（声道数）

```C++
typedef enum {
    AUDIO_CHANNEL_OUT_DISCRETE_CHANNEL_LIST_DEF(AUDIO_DEFINE_ENUM_SYMBOL_V)
    AUDIO_CHANNEL_IN_DISCRETE_CHANNEL_LIST_DEF(AUDIO_DEFINE_ENUM_SYMBOL_V)
    AUDIO_CHANNEL_IN_OUT_MASK_LIST_DEF(AUDIO_DEFINE_ENUM_SYMBOL_V)
    AUDIO_CHANNEL_OUT_MASK_LIST_DEF(AUDIO_DEFINE_ENUM_SYMBOL_V)
    AUDIO_CHANNEL_IN_MASK_LIST_DEF(AUDIO_DEFINE_ENUM_SYMBOL_V)
    AUDIO_CHANNEL_INDEX_MASK_LIST_DEF(AUDIO_DEFINE_ENUM_SYMBOL_V)
    AUDIO_CHANNEL_OUT_ALL =
        AUDIO_CHANNEL_OUT_DISCRETE_CHANNEL_LIST_DEF(AUDIO_DEFINE_BIT_MASK_V) 0,
    AUDIO_CHANNEL_HAPTIC_ALL = AUDIO_CHANNEL_OUT_HAPTIC_B |
                               AUDIO_CHANNEL_OUT_HAPTIC_A,
    AUDIO_CHANNEL_IN_ALL =
        AUDIO_CHANNEL_IN_DISCRETE_CHANNEL_LIST_DEF(AUDIO_DEFINE_BIT_MASK_V) 0,
    // This value must be part of the enum, but it is not a valid mask,
    // and thus it does not participate in to/from string conversions.
    AUDIO_CHANNEL_INVALID = 0xC0000000u,
} audio_channel_mask_t;
```

比较多，就不一一展开了，需要时查就好。

### 5.1.4 audio_output_flags_t（音频输出FLAG）

这个相对于java层是一个新的概念，定义了音频输出的方式。

```c++
typedef enum {
    AUDIO_OUTPUT_FLAG_NONE = 0x0
    AUDIO_OUTPUT_FLAG_DIRECT = 0x1
    AUDIO_OUTPUT_FLAG_PRIMARY = 0x2
    AUDIO_OUTPUT_FLAG_FAST = 0x4
    AUDIO_OUTPUT_FLAG_DEEP_BUFFER = 0x8
    AUDIO_OUTPUT_FLAG_COMPRESS_OFFLOAD = 0x10
    AUDIO_OUTPUT_FLAG_NON_BLOCKING = 0x20
    AUDIO_OUTPUT_FLAG_HW_AV_SYNC = 0x40
    AUDIO_OUTPUT_FLAG_TTS = 0x80
    AUDIO_OUTPUT_FLAG_RAW = 0x100
    AUDIO_OUTPUT_FLAG_SYNC = 0x200
    AUDIO_OUTPUT_FLAG_IEC958_NONAUDIO = 0x400
    AUDIO_OUTPUT_FLAG_DIRECT_PCM = 0x2000
    AUDIO_OUTPUT_FLAG_MMAP_NOIRQ = 0x4000
    AUDIO_OUTPUT_FLAG_VOIP_RX = 0x8000
    AUDIO_OUTPUT_FLAG_INCALL_MUSIC = 0x10000
} audio_output_flags_t;
```

部分FLAG含义如下：

| 名称                              | 含义                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| AUDIO_OUTPUT_FLAG_DIRECT          | 音频流不需要软件混音，直接交HAL处理，一般用于 HDMI 设备声音输出 |
| AUDIO_OUTPUT_FLAG_PRIMARY         | 表示音频流需要输出到主输出设备，一般用于铃声类声音           |
| AUDIO_OUTPUT_FLAG_FAST            | 表示音频流需要快速输出到音频设备，一般用于按键音、游戏背景音等对时延要求高的场景 |
| AUDIO_OUTPUT_FLAG_DEEP_BUFFER     | 表示音频流输出可以接受较大的时延，一般用于音乐、视频播放等对时延要求不高的场景 |
| AUDIO_OUTPUT_FLAG_COMPRES_OFFLOAD | 表示音频流没有经过软件解码，需要输出到硬件解码器，由硬件解码器进行解码 |

## 5.2 关键成员

| 成员类型              | 成员名称                   | 含义                            |
| --------------------- | -------------------------- | ------------------------------- |
| audio_stream_type_t   | mStreamType                | 流类型                          |
| audio_format_t        | mFormat                    | 位宽                            |
| audio_channel_mask_t  | mChannelMask               | 声道数mask                      |
| uint32_t              | mChannelCount              | 声道数                          |
| mutable uint32_t      | mSampleRate                | 采样率                          |
| uint32_t              | mOriginalSampleRate        | 初始采样率                      |
| audio_output_flags_t  | mFlags                     | 输出Flag                        |
| audio_output_flags_t  | mOrigFlags                 | 初始输出Flag                    |
| std::string           | mCallerName                | 调用方                          |
| bool                  | mThreadCanCallJava         | 是否是走JNI过来的               |
| audio_port_handle_t   | mSelectedDeviceId          | 输出设备id                      |
| transfer_type         | mTransfer                  | 数据如何传递给AudioTrack        |
| sp<IMemory>           | mSharedBuffer              | 共享内存（仅MODE_STATIC）       |
| bool                  | mDoNotReconnect            | 自动重连                        |
| sp<IAudioTrack>       | mAudioTrack                | 与AudioFlinger进行交互的binder  |
| size_t                | mFrameSize                 | 每个采样点的大小，单位：字节    |
| AudioPlaybackRate     | mPlaybackRate              | 播放速度/音高等与混音相关的属性 |
| audio_offload_info_t  | mOffloadInfoCopy           | offload信息的本地拷贝           |
| audio_offload_info_t* | mOffloadInfo               | offload信息                     |
| float                 | mVolume[2]                 | 音量                            |
| float                 | mSendLevel                 |                                 |
| size_t                | mReqFrameCount             | 缓冲区大小                      |
| uint32_t              | mNotificationFramesReq     | 每两次回调之间需要的音频帧数量  |
| uint32_t              | mNotificationsPerBufferReq | 每个buffer需要通知的数量        |
| uint32_t              | mNotificationFramesAct     | 每两次回调之间实际的音频帧数量  |
| uid_t                 | mClientUid                 | 调用方的uid                     |
| pid_t                 | mClientPid                 | 调用方的pid                     |





## 5.3 关键函数

### 5.3.1 setCallerName()

```C++
void setCallerName(const std::string &name)
```

赋值给mCallerName成员，代表本AudioTrack是什么地方调用的，“media”“AAudio”等

### 5.3.2 getCallerName()

```c++
std::string getCallerName()
```

返回mCallerName



# 6. AudioTrack创建源码

```c++
AudioTrack::AudioTrack(
        audio_stream_type_t streamType,			// 流类型
        uint32_t sampleRate,					// 采样率
        audio_format_t format,					// 位宽
        audio_channel_mask_t channelMask,		// 声道数
        const sp<IMemory>& sharedBuffer,		// 共享内存缓冲区：数据模式是 MODE_STATIC 时使用，数据模式是 MODE_STREAM 时为空
        audio_output_flags_t flags,				// 音频输出flag
        callback_t cbf,							// 回调
        void* user,								// TODO
        int32_t notificationFrames,				
        audio_session_t sessionId,
        transfer_type transferType,
        const audio_offload_info_t *offloadInfo,
        uid_t uid,
        pid_t pid,
        const audio_attributes_t* pAttributes,
        bool doNotReconnect,
        float maxRequiredSpeed,
        const std::string& opPackageName)
    : mStatus(NO_INIT),
      mState(STATE_STOPPED),
      mPreviousPriority(ANDROID_PRIORITY_NORMAL),
      mPreviousSchedulingGroup(SP_DEFAULT),
      mPausedPosition(0),
      mSelectedDeviceId(AUDIO_PORT_HANDLE_NONE),
      mOpPackageName(opPackageName),
      mAudioTrackCallback(new AudioTrackCallback())
{
    mAttributes = AUDIO_ATTRIBUTES_INITIALIZER;

    (void)set(streamType, sampleRate, format, channelMask,
            0 /*frameCount*/, flags, cbf, user, notificationFrames,
            sharedBuffer, false /*threadCanCallJava*/, sessionId, transferType, offloadInfo,
            uid, pid, pAttributes, doNotReconnect, maxRequiredSpeed);
}
```

## 6.1 关键Binder接口

AudioTrack.cpp与audioserver之间的关键Binder及定义位置如下：

| 类           | 位置                                        |
| ------------ | ------------------------------------------- |
| IAudioTrack  | IAudioTrack.h                               |
| BnAudioTrack | (定义)IAudioTrack.h / (实现)IAudioTrack.cpp |
| TrackHandle  | (定义)AudioFlinger.h / AudioFlinger.cpp     |
| BpAudioTrack | IAudioTrack.cpp                             |



| 类             | 位置                                            |
| -------------- | ----------------------------------------------- |
| IAudioFlinger  | IAudioFlinger.h                                 |
| BnAudioFlinger | (定义)IAudioFlinger.h / (实现)IAudioFlinger.cpp |
| AudioFlinger   | (定义)AudioFlinger.h / (实现)AudioFlinger.cpp   |
| BpAudioFlinger | IAudioFlinger.cpp                               |

IAudioTrack和IAudioFlinger都是实现在audioserver进程的audioflinger服务中的。 



| 类                   | 位置                                                        |
| -------------------- | ----------------------------------------------------------- |
| IAudioFlingerClient  | IAudioFlingerClient.h                                       |
| BnAudioFlingerClient | (定义)IAudioFlingerClient.h / (实现)IAudioFlingerClient.cpp |
| AudioFlingerClient   | (定义)AudioSystem.h / (实现)AudioSystem.cpp                 |
| BpAudioFlingerClient | IAudioFlingerClient.cpp                                     |

IAudioFlingerClient是客户端实现，用于AudioFlinger回调给AudioTrack

> 上面之所以列举出来这些位置是为了便于查询。实际上，真正重要的不是这些类定义在哪里，实现在哪里
>
> **最重要的是，客户端的Binder对象是哪里创建的**

下面给出第三张表：

| 类                 | 创建位置                                 |
| ------------------ | ---------------------------------------- |
| AudioFlinger       | 实名服务，在audioserver进程启动时创建    |
| TrackHandle        | audioserver进程（audioFlinger）中创建    |
| AudioFlingerClient | client进程（创建AudioTrack的进程）中创建 |



## 6.2 初始化列表

看下AudioTrack创建时的初始化列表

```c++
    : mStatus(NO_INIT),
      mState(STATE_STOPPED),
      mPreviousPriority(ANDROID_PRIORITY_NORMAL),
      mPreviousSchedulingGroup(SP_DEFAULT),
      mPausedPosition(0),
      mSelectedDeviceId(AUDIO_PORT_HANDLE_NONE),
      mOpPackageName(opPackageName),
      mAudioTrackCallback(new AudioTrackCallback())
```

### 6.2.1 mStatus和mState

mStatus和mState是描述AudioTrack状态的两个成员：

```c++
	status_t mStatus;
```

mStatus，类型为status_t，是android用于描述状态的一个通用类型。例如AudioTrack::set()，AudioTrack::createTrack()的返回值都是status_t类型。一般情况下，在返回值为status_t的函数开头，或者有status_t类型的类创建时，都要进行初始化。

### 6.2.2 mState

```c++
    enum State {
        STATE_ACTIVE,		// 激活
        STATE_STOPPED,		// 停止
        STATE_PAUSED,		// 暂停
        STATE_PAUSED_STOPPING,
        STATE_FLUSHED,
        STATE_STOPPING,
    } mState;
```

默认值是STATE_STOPPED

## 6.3 set()

AudioTrack的构造函数调用set()函数：

```C++
status_t AudioTrack::set(
        audio_stream_type_t streamType,
        uint32_t sampleRate,
        audio_format_t format,
        audio_channel_mask_t channelMask,
        size_t frameCount,
        audio_output_flags_t flags,
        callback_t cbf,
        void* user,
        int32_t notificationFrames,
        const sp<IMemory>& sharedBuffer,
        bool threadCanCallJava,
        audio_session_t sessionId,
        transfer_type transferType,
        const audio_offload_info_t *offloadInfo,
        uid_t uid,
        pid_t pid,
        const audio_attributes_t* pAttributes,
        bool doNotReconnect,
        float maxRequiredSpeed,
        audio_port_handle_t selectedDeviceId)
{
    
    // step 1 : 初始化mThreadCanCallJava、mSelectedDeviceId、mSessionId
    status_t status;
    uint32_t channelCount;
    pid_t callingPid;
    pid_t myPid;

    // Note mPortId is not valid until the track is created, so omit mPortId in ALOG for set.
    ALOGV("%s(): streamType %d, sampleRate %u, format %#x, channelMask %#x, frameCount %zu, "
          "flags #%x, notificationFrames %d, sessionId %d, transferType %d, uid %d, pid %d",
          __func__,
          streamType, sampleRate, format, channelMask, frameCount, flags, notificationFrames,
          sessionId, transferType, uid, pid);

    mThreadCanCallJava = threadCanCallJava;
    mSelectedDeviceId = selectedDeviceId;
    mSessionId = sessionId;

    // step 2 : 处理下transferType相关的一些异常分支，然后初始化mSharedBuffer，mTransfer和mDoNotReconnect
    switch (transferType) {
    case TRANSFER_DEFAULT:
        if (sharedBuffer != 0) {
            transferType = TRANSFER_SHARED;
        } else if (cbf == NULL || threadCanCallJava) {
            transferType = TRANSFER_SYNC;
        } else {
            transferType = TRANSFER_CALLBACK;
        }
        break;
    case TRANSFER_CALLBACK:
    case TRANSFER_SYNC_NOTIF_CALLBACK:
        if (cbf == NULL || sharedBuffer != 0) {
            ALOGE("%s(): Transfer type %s but cbf == NULL || sharedBuffer != 0",
                    convertTransferToText(transferType), __func__);
            status = BAD_VALUE;
            goto exit;
        }
        break;
    case TRANSFER_OBTAIN:
    case TRANSFER_SYNC:
        if (sharedBuffer != 0) {
            ALOGE("%s(): Transfer type TRANSFER_OBTAIN but sharedBuffer != 0", __func__);
            status = BAD_VALUE;
            goto exit;
        }
        break;
    case TRANSFER_SHARED:
        if (sharedBuffer == 0) {
            ALOGE("%s(): Transfer type TRANSFER_SHARED but sharedBuffer == 0", __func__);
            status = BAD_VALUE;
            goto exit;
        }
        break;
    default:
        ALOGE("%s(): Invalid transfer type %d",
                __func__, transferType);
        status = BAD_VALUE;
        goto exit;
    }

    mSharedBuffer = sharedBuffer;
    mTransfer = transferType;
    mDoNotReconnect = doNotReconnect;

    ALOGV_IF(sharedBuffer != 0, "%s(): sharedBuffer: %p, size: %zu",
            __func__, sharedBuffer->unsecurePointer(), sharedBuffer->size());

    ALOGV("%s(): streamType %d frameCount %zu flags %04x",
            __func__, streamType, frameCount, flags);

    // step 3 : 处理mAudioTrack的异常场景
    // invariant that mAudioTrack != 0 is true only after set() returns successfully
    if (mAudioTrack != 0) {
        ALOGE("%s(): Track already in use", __func__);
        status = INVALID_OPERATION;
        goto exit;
    }

    // step 4 : 初始化mStreamType
    // handle default values first.
    if (streamType == AUDIO_STREAM_DEFAULT) {
        streamType = AUDIO_STREAM_MUSIC;
    }

    if (pAttributes == NULL) {
        if (uint32_t(streamType) >= AUDIO_STREAM_PUBLIC_CNT) {
            ALOGE("%s(): Invalid stream type %d", __func__, streamType);
            status = BAD_VALUE;
            goto exit;
        }
        mStreamType = streamType;

    } else {
        // stream type shouldn't be looked at, this track has audio attributes
        memcpy(&mAttributes, pAttributes, sizeof(audio_attributes_t));
        ALOGV("%s(): Building AudioTrack with attributes:"
                " usage=%d content=%d flags=0x%x tags=[%s]",
                __func__,
                 mAttributes.usage, mAttributes.content_type, mAttributes.flags, mAttributes.tags);
        mStreamType = AUDIO_STREAM_DEFAULT;
        audio_flags_to_audio_output_flags(mAttributes.flags, &flags);
    }

    // step 5 : 初始化mFormat
    // these below should probably come from the audioFlinger too...
    if (format == AUDIO_FORMAT_DEFAULT) {
        format = AUDIO_FORMAT_PCM_16_BIT;
    } else if (format == AUDIO_FORMAT_IEC61937) { // HDMI pass-through?
        flags = static_cast<audio_output_flags_t>(flags | AUDIO_OUTPUT_FLAG_IEC958_NONAUDIO);
    }

    // validate parameters
    if (!audio_is_valid_format(format)) {
        ALOGE("%s(): Invalid format %#x", __func__, format);
        status = BAD_VALUE;
        goto exit;
    }

    mFormat = format;

    // step 6 : 初始化mChannelMask和mChannelCount
    if (!audio_is_output_channel(channelMask)) {
        ALOGE("%s(): Invalid channel mask %#x",  __func__, channelMask);
        status = BAD_VALUE;
        goto exit;
    }

    mChannelMask = channelMask;
    channelCount = audio_channel_count_from_out_mask(channelMask);
    mChannelCount = channelCount;

    // step 7 : 进一步调整flags
    // force direct flag if format is not linear PCM
    // or offload was requested
    if ((flags & AUDIO_OUTPUT_FLAG_COMPRESS_OFFLOAD)
            || !audio_is_linear_pcm(format)) {
        ALOGV( (flags & AUDIO_OUTPUT_FLAG_COMPRESS_OFFLOAD)
                    ? "%s(): Offload request, forcing to Direct Output"
                    : "%s(): Not linear PCM, forcing to Direct Output",
                    __func__);
        flags = (audio_output_flags_t)
                // FIXME why can't we allow direct AND fast?
                ((flags | AUDIO_OUTPUT_FLAG_DIRECT) & ~AUDIO_OUTPUT_FLAG_FAST);
    }

    // force direct flag if HW A/V sync requested
    if ((flags & AUDIO_OUTPUT_FLAG_HW_AV_SYNC) != 0) {
        flags = (audio_output_flags_t)(flags | AUDIO_OUTPUT_FLAG_DIRECT);
    }

    // step 8 : 初始化mFrameSize
    if (flags & AUDIO_OUTPUT_FLAG_DIRECT) {
        if (audio_has_proportional_frames(format)) {
            mFrameSize = channelCount * audio_bytes_per_sample(format);
        } else {
            mFrameSize = sizeof(uint8_t);
        }
    } else {
        ALOG_ASSERT(audio_has_proportional_frames(format));
        mFrameSize = channelCount * audio_bytes_per_sample(format);
        // createTrack will return an error if PCM format is not supported by server,
        // so no need to check for specific PCM formats here
    }

    // step 9 : 初始化mSampleRate，mOriginalSampleRate，mPlaybackRate和mMaxRequiredSpeed
    // sampling rate must be specified for direct outputs
    if (sampleRate == 0 && (flags & AUDIO_OUTPUT_FLAG_DIRECT) != 0) {
        status = BAD_VALUE;
        goto exit;
    }

    mSampleRate = sampleRate;
    mOriginalSampleRate = sampleRate;
    mPlaybackRate = AUDIO_PLAYBACK_RATE_DEFAULT;
    // 1.0 <= mMaxRequiredSpeed <= AUDIO_TIMESTRETCH_SPEED_MAX
    mMaxRequiredSpeed = min(max(maxRequiredSpeed, 1.0f), AUDIO_TIMESTRETCH_SPEED_MAX);

    // step 10 : 初始化mOffloadInfoCopy、mOffloadInfo
    // Make copy of input parameter offloadInfo so that in the future:
    //  (a) createTrack_l doesn't need it as an input parameter
    //  (b) we can support re-creation of offloaded tracks
    if (offloadInfo != NULL) {
        mOffloadInfoCopy = *offloadInfo;
        mOffloadInfo = &mOffloadInfoCopy;
    } else {
        mOffloadInfo = NULL;
        memset(&mOffloadInfoCopy, 0, sizeof(audio_offload_info_t));
    }

    // step 11 : 初始化mVolume、mSendLevel和mReqFrameCount
    mVolume[AUDIO_INTERLEAVE_LEFT] = 1.0f;
    mVolume[AUDIO_INTERLEAVE_RIGHT] = 1.0f;
    mSendLevel = 0.0f;
    // mFrameCount is initialized in createTrack_l
    mReqFrameCount = frameCount;
    
    // step 12 : 初始化mNotificationFramesReq、mNotificationsPerBufferReq和mNotificationFramesAct
    if (notificationFrames >= 0) {
        mNotificationFramesReq = notificationFrames;
        mNotificationsPerBufferReq = 0;
    } else {
        if (!(flags & AUDIO_OUTPUT_FLAG_FAST)) {
            ALOGE("%s(): notificationFrames=%d not permitted for non-fast track",
                    __func__, notificationFrames);
            status = BAD_VALUE;
            goto exit;
        }
        if (frameCount > 0) {
            ALOGE("%s(): notificationFrames=%d not permitted with non-zero frameCount=%zu",
                    __func__, notificationFrames, frameCount);
            status = BAD_VALUE;
            goto exit;
        }
        mNotificationFramesReq = 0;
        const uint32_t minNotificationsPerBuffer = 1;
        const uint32_t maxNotificationsPerBuffer = 8;
        mNotificationsPerBufferReq = min(maxNotificationsPerBuffer,
                max((uint32_t) -notificationFrames, minNotificationsPerBuffer));
        ALOGW_IF(mNotificationsPerBufferReq != (uint32_t) -notificationFrames,
                "%s(): notificationFrames=%d clamped to the range -%u to -%u",
                __func__,
                notificationFrames, minNotificationsPerBuffer, maxNotificationsPerBuffer);
    }
    mNotificationFramesAct = 0;
    
    // step 13 : 初始化mClientPid和mClientUid
    callingPid = IPCThreadState::self()->getCallingPid();
    myPid = getpid();
    if (uid == AUDIO_UID_INVALID || (callingPid != myPid)) {
        mClientUid = IPCThreadState::self()->getCallingUid();
    } else {
        mClientUid = uid;
    }
    if (pid == -1 || (callingPid != myPid)) {
        mClientPid = callingPid;
    } else {
        mClientPid = pid;
    }
    
    // step 14 : 初始化mAuxEffected、mOrigFlags、mCbf
    mAuxEffectId = 0;
    mOrigFlags = mFlags = flags;
    mCbf = cbf;

    // step 15 : 启动AudioTrackThread
    if (cbf != NULL) {
        mAudioTrackThread = new AudioTrackThread(*this);
        mAudioTrackThread->run("AudioTrack", ANDROID_PRIORITY_AUDIO, 0 /*stack*/);
        // thread begins in paused state, and will not reference us until start()
    }

    // step 16 : 创建IAudioTrack
    // create the IAudioTrack
    {
        AutoMutex lock(mLock);
        status = createTrack_l();
    }

    if (status != NO_ERROR) {
        if (mAudioTrackThread != 0) {
            mAudioTrackThread->requestExit();   // see comment in AudioTrack.h
            mAudioTrackThread->requestExitAndWait();
            mAudioTrackThread.clear();
        }
        goto exit;
    }

    // step 17 : 其他成员的初始化
    mUserData = user;
    mLoopCount = 0;
    mLoopStart = 0;
    mLoopEnd = 0;
    mLoopCountNotified = 0;
    mMarkerPosition = 0;
    mMarkerReached = false;
    mNewPosition = 0;
    mUpdatePeriod = 0;
    mPosition = 0;
    mReleased = 0;
    mStartNs = 0;
    mStartFromZeroUs = 0;
    AudioSystem::acquireAudioSessionId(mSessionId, mClientPid, mClientUid);
    mSequence = 1;
    mObservedSequence = mSequence;
    mInUnderrun = false;
    mPreviousTimestampValid = false;
    mTimestampStartupGlitchReported = false;
    mTimestampRetrogradePositionReported = false;
    mTimestampRetrogradeTimeReported = false;
    mTimestampStallReported = false;
    mTimestampStaleTimeReported = false;
    mPreviousLocation = ExtendedTimestamp::LOCATION_INVALID;
    mStartTs.mPosition = 0;
    mUnderrunCountOffset = 0;
    mFramesWritten = 0;
    mFramesWrittenServerOffset = 0;
    mFramesWrittenAtRestore = -1; // -1 is a unique initializer.
    mVolumeHandler = new media::VolumeHandler();

exit:
    mStatus = status;
    return status;
}
```

这个函数很长，同时也是非常重要的一个函数。细化为step1~step25：

### 6.3.1 step 1

**初始化mThreadCanCallJava、mSelectedDeviceId、mSessionId**

```c++
    // step 1
    status_t status;
    uint32_t channelCount;
    pid_t callingPid;
    pid_t myPid;

    // Note mPortId is not valid until the track is created, so omit mPortId in ALOG for set.
    ALOGV("%s(): streamType %d, sampleRate %u, format %#x, channelMask %#x, frameCount %zu, "
          "flags #%x, notificationFrames %d, sessionId %d, transferType %d, uid %d, pid %d",
          __func__,
          streamType, sampleRate, format, channelMask, frameCount, flags, notificationFrames,
          sessionId, transferType, uid, pid);

    mThreadCanCallJava = threadCanCallJava;
    mSelectedDeviceId = selectedDeviceId;
    mSessionId = sessionId;

```

查看`status_t`的定义：

> /system/core/libutils/include/utils/Errors.h
>
> libutils.so

```C++
typedef int32_t status_t;
```

这是linux定义的专门用于标识函数返回状态码的数据类型，可能的值有（仅列举部分）：

```C++
enum {
	OK = 0,
	NO_ERROR = OK,
	BAD_VALUE = -EINVAL,
	...	
}
```

重点看下最下面的三行赋值语句：

```c++
    mThreadCanCallJava = threadCanCallJava;
    mSelectedDeviceId = selectedDeviceId;
    mSessionId = sessionId;
```

#### 6.3.1.1 mThreadCanCallJava

```c++
bool mThreadCanCallJava
```

从字面意思看，这个成员标识这是不是一个java线程，或者是不是通过jni调过来的。

> 1、通过MediaPlayer创建AudioTrack时，该值为false
>
> 2、通过AudioTrack.java创建AudioTrack时，该值为true

该值会影响到linux进程调度的策略。

#### 6.3.1.2 mSelectedDeviceId

```c++
audio_port_handle_t selectedDeviceId
```

先看下这个数据类型

> audio.h

```c++
/* Each port has a unique ID or handle allocated by policy manager */
typedef int audio_port_handle_t;
```

emm，这也是个int型，C++通过这种重定义，可以方便理解其含义。

接下来看下MediaPlayer和JNI分别是怎么传入selectedDeviceId的。

> 1、 MediaPlayer把mSelectedDeviceId成员传入
>
> 看下MediaPlayerService的mSelectedDeviceId的默认值和赋值：

```c++
mSelectedDeviceId(AUDIO_PORT_HANDLE_NONE),

status_t MediaPlayerService::AudioOutput::setOutputDevice(audio_port_handle_t deviceId)
{
    ALOGV("setOutputDevice(%d)", deviceId);
    Mutex::Autolock lock(mLock);
    mSelectedDeviceId = deviceId;
    if (mTrack != 0) {
        return mTrack->setOutputDevice(deviceId);
    }
    return NO_ERROR;
}
```

> AUDIO_PORT_HANDLE_NONE定义在audio.h

```c++
/* Null values for handles. */
enum {
    AUDIO_IO_HANDLE_NONE = 0,
    AUDIO_MODULE_HANDLE_NONE = 0,
    AUDIO_PORT_HANDLE_NONE = 0,
    AUDIO_PATCH_HANDLE_NONE = 0,
};
```

> 2、JNI，来到android_media_AudioTrack.cpp

```c++
// 只看核心代码
sp<AudioTrack> lpTrack;

lpTrack = new AudioTrack(opPackageNameStr.c_str());

// read the AudioAttributes values
auto paa = JNIAudioAttributeHelper::makeUnique();
// 这里的jaa代表从java层传过来的AudioAttributes
jint jStatus = JNIAudioAttributeHelper::nativeFromJava(env, jaa, paa.get());


            status = lpTrack->set(AUDIO_STREAM_DEFAULT, // stream type, but more info conveyed
                                                        // in paa (last argument)
                                  sampleRateInHertz,
                                  format, // word length, PCM
                                  nativeChannelMask, frameCount, AUDIO_OUTPUT_FLAG_NONE,
                                  audioCallback,
                                  &(lpJniStorage->mCallbackData), // callback, callback data (user)
                                  0, // notificationFrames == 0 since not using EVENT_MORE_DATA
                                     // to feed the AudioTrack
                                  lpJniStorage->mMemBase, // shared mem
                                  true,                   // thread can call Java
                                  sessionId,              // audio session ID
                                  AudioTrack::TRANSFER_SHARED,
                                  NULL,   // default offloadInfo
                                  -1, -1, // default uid, pid values
                                  paa.get());
```

> 注意，这里的set()只传了17个参数，而AudioTrack.h中定义的set()是有20个参数的。原因在于set()设置了默认参数值。

> AudioTrack.h

```c++
            status_t    set(audio_stream_type_t streamType,
                            uint32_t sampleRate,
                            audio_format_t format,
                            uint32_t channelMask,
                            size_t frameCount   = 0,
                            audio_output_flags_t flags = AUDIO_OUTPUT_FLAG_NONE,
                            callback_t cbf      = NULL,
                            void* user          = NULL,
                            int32_t notificationFrames = 0,
                            const sp<IMemory>& sharedBuffer = 0,
                            bool threadCanCallJava = false,
                            audio_session_t sessionId  = AUDIO_SESSION_ALLOCATE,
                            transfer_type transferType = TRANSFER_DEFAULT,
                            const audio_offload_info_t *offloadInfo = NULL,
                            uid_t uid = AUDIO_UID_INVALID,
                            pid_t pid = -1,
                            const audio_attributes_t* pAttributes = NULL,
                            bool doNotReconnect = false,
                            float maxRequiredSpeed = 1.0f,
                            // 默认值是AUDIO_PORT_HANDLE_NONE
                            audio_port_handle_t selectedDeviceId = AUDIO_PORT_HANDLE_NONE);
```

综上，mSelectedDeviceId默认值是AUDIO_PORT_HANDLE_NONE，在MediaPlayerService中对外暴露setOutputDevice可以修改该值。猜测该值用于设置输出设备id。

#### 6.3.1.3 mSessionId

```c++
audio_session_t sessionId
```

查看audio_session_t的定义：

> audio.h

```c++
typedef enum {
    AUDIO_SESSION_DEVICE = HAL_AUDIO_SESSION_DEVICE,
    AUDIO_SESSION_OUTPUT_STAGE = HAL_AUDIO_SESSION_OUTPUT_STAGE,
    AUDIO_SESSION_OUTPUT_MIX = HAL_AUDIO_SESSION_OUTPUT_MIX,
#ifndef AUDIO_NO_SYSTEM_DECLARATIONS
    AUDIO_SESSION_ALLOCATE = 0,
    AUDIO_SESSION_NONE = 0,
#endif
} audio_session_t;
```

> 1、查看AudioTrack.h，其缺省值为AUDIO_SESSION_ALLOCATE，也就是0。
>
> 2、MediaPlayerService中，是创建AudioOutput时传进来的
>
> 3、JNI中，是从java层传进来的，一般也是0

### 6.3.2 step 2

**处理下transferType相关的一些异常分支，然后初始化mSharedBuffer，mTransfer和mDoNotReconnect**

```c++
    // step 2
    switch (transferType) {
    case TRANSFER_DEFAULT:
        if (sharedBuffer != 0) {
            transferType = TRANSFER_SHARED;
        } else if (cbf == NULL || threadCanCallJava) {
            transferType = TRANSFER_SYNC;
        } else {
            transferType = TRANSFER_CALLBACK;
        }
        break;
    case TRANSFER_CALLBACK:
    case TRANSFER_SYNC_NOTIF_CALLBACK:
        if (cbf == NULL || sharedBuffer != 0) {
            ALOGE("%s(): Transfer type %s but cbf == NULL || sharedBuffer != 0",
                    convertTransferToText(transferType), __func__);
            status = BAD_VALUE;
            goto exit;
        }
        break;
    case TRANSFER_OBTAIN:
    case TRANSFER_SYNC:
        if (sharedBuffer != 0) {
            ALOGE("%s(): Transfer type TRANSFER_OBTAIN but sharedBuffer != 0", __func__);
            status = BAD_VALUE;
            goto exit;
        }
        break;
    case TRANSFER_SHARED:
        if (sharedBuffer == 0) {
            ALOGE("%s(): Transfer type TRANSFER_SHARED but sharedBuffer == 0", __func__);
            status = BAD_VALUE;
            goto exit;
        }
        break;
    default:
        ALOGE("%s(): Invalid transfer type %d",
                __func__, transferType);
        status = BAD_VALUE;
        goto exit;
    }
    mSharedBuffer = sharedBuffer;
    mTransfer = transferType;
    mDoNotReconnect = doNotReconnect;

    ALOGV_IF(sharedBuffer != 0, "%s(): sharedBuffer: %p, size: %zu",
            __func__, sharedBuffer->unsecurePointer(), sharedBuffer->size());

    ALOGV("%s(): streamType %d frameCount %zu flags %04x",
            __func__, streamType, frameCount, flags);
```

#### 6.3.2.1 mTransfer

先看下入参：transferType，这个值标识了数据如何传递给AudioTrack

```c++
transfer_type transferType,
```

transfer_type定义：

> AudioTrack.h

```c++
    /* How data is transferred to AudioTrack
     */
    enum transfer_type {
        TRANSFER_DEFAULT,   // not specified explicitly; determine from the other parameters
        TRANSFER_CALLBACK,  // callback EVENT_MORE_DATA
        TRANSFER_OBTAIN,    // call obtainBuffer() and releaseBuffer()
        TRANSFER_SYNC,      // synchronous write()
        TRANSFER_SHARED,    // shared memory
        TRANSFER_SYNC_NOTIF_CALLBACK, // synchronous write(), notif EVENT_CAN_WRITE_MORE_DATA
    };
```

注释翻译下：

| transfer_type                | 含义                                         |
| ---------------------------- | -------------------------------------------- |
| TRANSFER_DEFAULT             | 未明确指定，通过其他参数决定传输类型         |
| TRANSFER_CALLBACK            | 回调EVENT_MORE_DATA                          |
| TRANSFER_OBTAIN              | 调用obtainBuffer()和releaseBuffer()          |
| TRANSFER_SYNC                | 同步的write()                                |
| TRANSFER_SHARED              | 共享内存                                     |
| TRANSFER_SYNC_NOTIF_CALLBACK | 同步write()，并通知EVENT_CAN_WRITE_MORE_DATA |

> transferType会经过一系列的处理重新赋值，最后赋值给mTransfer
>
> 1、AudioTrack.h中的缺省值为：TRANSFER_DEFAULT
>
> 2、MediaPlayerService中，当mCallback != null时，transferType为TRANSFER_CALLBACK，否则为TRANSFER_DEFAULT
>
> 3、JNI中，要综合判断是offload模式，以及mode的值
>
> ```c++
> case MODE_STREAM:
> 	offload ? AudioTrack::TRANSFER_SYNC_NOTIF_CALLBACK
>                                           : AudioTrack::TRANSFER_SYNC,
> 
> case MODE_STATIC:
> 	AudioTrack::TRANSFER_SHARED,
> ```

#### 6.3.2.2 mSharedBuffer

再看下入参sharedBuffer

```c++
const sp<IMemory>& sharedBuffer
```

> 最终sharedBuffer赋值给mSharedBuffer成员：
>
> 1、AudioTrack.h中的缺省值为：0
>
> 2、MediaPlayerService没有传sharedBuffer，所以采用缺省值
>
> 3、JNI在MODE_STREAM下传0，在MODE_STATIC下传共享内存

#### 6.3.2.3 mDoNotReconnect

```c++
bool mDoNotReconnect;
```

> 1、AudioTrack.h中，缺省值为：false
>
> 2、MediaPlayerService中，同样按缺省值：false
>
> 3、JNI中，缺省值：false

#### 6.3.2.4 走读

再梳理下逻辑：

```c++
    // step 2
    switch (transferType) {
    case TRANSFER_DEFAULT:
        // TRANSFER_DEFAULT会根据其他的参数决定最终的transfer类型
        // 1、 sharedBuffer不为0时，意味着走MODE_STATIC,transferType设置为TRANSFER_SHARED
        // 2、 cbf（回调）为空，或者是JNI调用的场景，走TRANSFER_SYNC
            // 这里插一句，6.2.2.1已经看过代码，JNI的场景下不可能传TRANSFER_DEFAULT的
        // 3、 有cbf时，走TRANSFER_CALLBACK
        if (sharedBuffer != 0) {
            transferType = TRANSFER_SHARED;
        } else if (cbf == NULL || threadCanCallJava) {
            transferType = TRANSFER_SYNC;
        } else {
            transferType = TRANSFER_CALLBACK;
        }
        break;
    case TRANSFER_CALLBACK:
    case TRANSFER_SYNC_NOTIF_CALLBACK:
        // transferType是CALLBACK时，回调不能是NULL，且不能走MODE_STATIC
        // 可以理解为选择走回调的方式了，callback就不能为空了;MODE_STATIC模式必须走TRANSFER_SHARED
        if (cbf == NULL || sharedBuffer != 0) {
            ALOGE("%s(): Transfer type %s but cbf == NULL || sharedBuffer != 0",
                    convertTransferToText(transferType), __func__);
            status = BAD_VALUE;
            goto exit;
        }
        break;
    case TRANSFER_OBTAIN:
    case TRANSFER_SYNC:
        // MODE_STATIC必须走TRANSFER_SHARED
        if (sharedBuffer != 0) {
            ALOGE("%s(): Transfer type TRANSFER_OBTAIN but sharedBuffer != 0", __func__);
            status = BAD_VALUE;
            goto exit;
        }
        break;
    case TRANSFER_SHARED:
        // MODE_STATIC必须有sharedBuffer
        if (sharedBuffer == 0) {
            ALOGE("%s(): Transfer type TRANSFER_SHARED but sharedBuffer == 0", __func__);
            status = BAD_VALUE;
            goto exit;
        }
        break;
    default:
        ALOGE("%s(): Invalid transfer type %d",
                __func__, transferType);
        status = BAD_VALUE;
        goto exit;
    }

	// 异常场景会goto到函数末尾

	// 赋值给mSharedBuffer
    mSharedBuffer = sharedBuffer;

	// 赋值给mTransfer
    mTransfer = transferType;

	// 赋值给mDoNotReconnect
    mDoNotReconnect = doNotReconnect;

    ALOGV_IF(sharedBuffer != 0, "%s(): sharedBuffer: %p, size: %zu",
            __func__, sharedBuffer->unsecurePointer(), sharedBuffer->size());

    ALOGV("%s(): streamType %d frameCount %zu flags %04x",
            __func__, streamType, frameCount, flags);
```

### 6.3.3 step 3

**处理mAudioTrack的异常场景**

```c++
    // invariant that mAudioTrack != 0 is true only after set() returns successfully
    if (mAudioTrack != 0) {
        ALOGE("%s(): Track already in use", __func__);
        status = INVALID_OPERATION;
        goto exit;
    }
```

mAudioTrack是AudioTrack.cpp中，用于与AudioFlinger进行交互的重要Binder代理。如果set()函数调用时，mAudioTrack已经创建好了，视为异常场景，直接退出。

### 6.3.4 step 4

**初始化mStreamType**

mStreamType的取值范围参考5.1.1节

```c++
    // handle default values first.
    if (streamType == AUDIO_STREAM_DEFAULT) {
        streamType = AUDIO_STREAM_MUSIC;
    }

	if (pAttributes == NULL) {
        if (uint32_t(streamType) >= AUDIO_STREAM_PUBLIC_CNT) {
            ALOGE("%s(): Invalid stream type %d", __func__, streamType);
            status = BAD_VALUE;
            goto exit;
        }
        mStreamType = streamType;

    } else {
        // stream type shouldn't be looked at, this track has audio attributes
        memcpy(&mAttributes, pAttributes, sizeof(audio_attributes_t));
        ALOGV("%s(): Building AudioTrack with attributes:"
                " usage=%d content=%d flags=0x%x tags=[%s]",
                __func__,
                 mAttributes.usage, mAttributes.content_type, mAttributes.flags, mAttributes.tags);
        mStreamType = AUDIO_STREAM_DEFAULT;
        audio_flags_to_audio_output_flags(mAttributes.flags, &flags);
    }
```

#### 6.3.4.1 pAttributes

```c++
const audio_attributes_t* pAttributes
```

看下audio_attributes_t的定义：

> audio.h

```c++
typedef struct {
    audio_content_type_t content_type;
    audio_usage_t        usage;
    audio_source_t       source;
    audio_flags_mask_t   flags;
    char                 tags[AUDIO_ATTRIBUTES_TAGS_MAX_SIZE]; /* UTF8 */
} __attribute__((packed)) audio_attributes_t; // sent through Binder;
```

> 1、 AudioTrack.h中，pAttributes默认为NULL
>
> 2、 MediaPlayerService中，传入mAttributes成员
>
> 3、 JNI中，从java层的AudioAttributes对象中拿到

注意到创建AudioTrack时，是不一定会创建AudioAttributes的，所以pAttributes可能为NULL

注意到audio_attributes_t中，有一个audio_flags_mask_t，其可能的值有：

```c++
typedef enum {
    AUDIO_FLAG_NONE                       = 0x0,
    AUDIO_FLAG_AUDIBILITY_ENFORCED        = 0x1,
    AUDIO_FLAG_SECURE                     = 0x2,
    AUDIO_FLAG_SCO                        = 0x4,
    AUDIO_FLAG_BEACON                     = 0x8,
    AUDIO_FLAG_HW_AV_SYNC                 = 0x10,
    AUDIO_FLAG_HW_HOTWORD                 = 0x20,
    AUDIO_FLAG_BYPASS_INTERRUPTION_POLICY = 0x40,
    AUDIO_FLAG_BYPASS_MUTE                = 0x80,
    AUDIO_FLAG_LOW_LATENCY                = 0x100,
    AUDIO_FLAG_DEEP_BUFFER                = 0x200,
    AUDIO_FLAG_NO_MEDIA_PROJECTION        = 0X400,
    AUDIO_FLAG_MUTE_HAPTIC                = 0x800,
    AUDIO_FLAG_NO_SYSTEM_CAPTURE          = 0X1000,
    AUDIO_FLAG_CAPTURE_PRIVATE            = 0X2000,
} audio_flags_mask_t;
```

#### 6.3.4.2 flags

```c++
audio_output_flags_t flags
```

```c++
typedef enum {
    AUDIO_OUTPUT_FLAG_NONE = 0x0
    AUDIO_OUTPUT_FLAG_DIRECT = 0x1
    AUDIO_OUTPUT_FLAG_PRIMARY = 0x2
    AUDIO_OUTPUT_FLAG_FAST = 0x4
    AUDIO_OUTPUT_FLAG_DEEP_BUFFER = 0x8
    AUDIO_OUTPUT_FLAG_COMPRESS_OFFLOAD = 0x10
    AUDIO_OUTPUT_FLAG_NON_BLOCKING = 0x20
    AUDIO_OUTPUT_FLAG_HW_AV_SYNC = 0x40
    AUDIO_OUTPUT_FLAG_TTS = 0x80
    AUDIO_OUTPUT_FLAG_RAW = 0x100
    AUDIO_OUTPUT_FLAG_SYNC = 0x200
    AUDIO_OUTPUT_FLAG_IEC958_NONAUDIO = 0x400
    AUDIO_OUTPUT_FLAG_DIRECT_PCM = 0x2000
    AUDIO_OUTPUT_FLAG_MMAP_NOIRQ = 0x4000
    AUDIO_OUTPUT_FLAG_VOIP_RX = 0x8000
    AUDIO_OUTPUT_FLAG_INCALL_MUSIC = 0x10000
} audio_output_flags_t;
```

可以看到这个flag跟pAttributes中的flags有所重叠，但不尽相同。

#### 6.3.4.3 走读

```c++
    // handle default values first.
    if (streamType == AUDIO_STREAM_DEFAULT) {
        streamType = AUDIO_STREAM_MUSIC;
    }

	// 如果pAttributes为NULL
	if (pAttributes == NULL) {
        // 异常检查
        if (uint32_t(streamType) >= AUDIO_STREAM_PUBLIC_CNT) {
            ALOGE("%s(): Invalid stream type %d", __func__, streamType);
            status = BAD_VALUE;
            goto exit;
        }
        // 赋值给mStreamType
        mStreamType = streamType;

    } else {
        // stream type shouldn't be looked at, this track has audio attributes
        // 可以理解为深拷贝了一份
        memcpy(&mAttributes, pAttributes, sizeof(audio_attributes_t));
        ALOGV("%s(): Building AudioTrack with attributes:"
                " usage=%d content=%d flags=0x%x tags=[%s]",
                __func__,
                 mAttributes.usage, mAttributes.content_type, mAttributes.flags, mAttributes.tags);
        // mStreamType设置为AUDIO_STREAM_DEFAULT
        mStreamType = AUDIO_STREAM_DEFAULT;
        
        // 
        audio_flags_to_audio_output_flags(mAttributes.flags, &flags);
    }
```

展开看下audio_flags_to_audio_output_flags()

```c++
// 这两个入参的flag，第一个来自于audio_attributes_t，也就是AudioAttributes，第二个来自于audio_output_flags_t
static inline void audio_flags_to_audio_output_flags(
                                           const audio_flags_mask_t audio_flags,
                                           audio_output_flags_t *flags)
{
    // 根据pAttributes中的flag，修改output flags的值
    if ((audio_flags & AUDIO_FLAG_HW_AV_SYNC) != 0) {
        // 这里多按位或一个，就会多一位设置为1
        *flags = (audio_output_flags_t)(*flags |
            AUDIO_OUTPUT_FLAG_HW_AV_SYNC | AUDIO_OUTPUT_FLAG_DIRECT);
    }
    if ((audio_flags & AUDIO_FLAG_LOW_LATENCY) != 0) {
        *flags = (audio_output_flags_t)(*flags | AUDIO_OUTPUT_FLAG_FAST);
    }
    // check deep buffer after flags have been modified above
    if (*flags == AUDIO_OUTPUT_FLAG_NONE && (audio_flags & AUDIO_FLAG_DEEP_BUFFER) != 0) {
        *flags = AUDIO_OUTPUT_FLAG_DEEP_BUFFER;
    }
}
```

### 6.3.5 step 5

**初始化mFormat**

mFormat的取值范围参考5.1.2节

```c++
    // these below should probably come from the audioFlinger too...
    if (format == AUDIO_FORMAT_DEFAULT) {
        // 没有format时，默认走16位
        format = AUDIO_FORMAT_PCM_16_BIT;
    } else if (format == AUDIO_FORMAT_IEC61937) { // HDMI pass-through?
        // 根据format调整flags
        flags = static_cast<audio_output_flags_t>(flags | AUDIO_OUTPUT_FLAG_IEC958_NONAUDIO);
    }

    // validate parameters
	// 校验format是否合规
    if (!audio_is_valid_format(format)) {
        ALOGE("%s(): Invalid format %#x", __func__, format);
        status = BAD_VALUE;
        goto exit;
    }

    mFormat = format;
```

对format的校验：

```C++
static inline bool audio_is_valid_format(audio_format_t format)
{
    switch (format & AUDIO_FORMAT_MAIN_MASK) {
    case AUDIO_FORMAT_PCM:
        switch (format) {
        case AUDIO_FORMAT_PCM_16_BIT:
        case AUDIO_FORMAT_PCM_8_BIT:
        case AUDIO_FORMAT_PCM_32_BIT:
        case AUDIO_FORMAT_PCM_8_24_BIT:
        case AUDIO_FORMAT_PCM_FLOAT:
        case AUDIO_FORMAT_PCM_24_BIT_PACKED:
            return true;
        default:
            return false;
        }
        /* not reached */
    case AUDIO_FORMAT_MP3:
    case AUDIO_FORMAT_AMR_NB:
    case AUDIO_FORMAT_AMR_WB:
        return true;
    case AUDIO_FORMAT_AAC:
        switch (format) {
        case AUDIO_FORMAT_AAC:
        case AUDIO_FORMAT_AAC_MAIN:
        case AUDIO_FORMAT_AAC_LC:
        case AUDIO_FORMAT_AAC_SSR:
        case AUDIO_FORMAT_AAC_LTP:
        case AUDIO_FORMAT_AAC_HE_V1:
        case AUDIO_FORMAT_AAC_SCALABLE:
        case AUDIO_FORMAT_AAC_ERLC:
        case AUDIO_FORMAT_AAC_LD:
        case AUDIO_FORMAT_AAC_HE_V2:
        case AUDIO_FORMAT_AAC_ELD:
        case AUDIO_FORMAT_AAC_XHE:
            return true;
        default:
            return false;
        }
        /* not reached */
    case AUDIO_FORMAT_HE_AAC_V1:
    case AUDIO_FORMAT_HE_AAC_V2:
    case AUDIO_FORMAT_VORBIS:
    case AUDIO_FORMAT_OPUS:
    case AUDIO_FORMAT_AC3:
        return true;
    case AUDIO_FORMAT_E_AC3:
        switch (format) {
        case AUDIO_FORMAT_E_AC3:
        case AUDIO_FORMAT_E_AC3_JOC:
            return true;
        default:
            return false;
        }
        /* not reached */
    case AUDIO_FORMAT_DTS:
    case AUDIO_FORMAT_DTS_HD:
    case AUDIO_FORMAT_IEC60958:
    case AUDIO_FORMAT_IEC61937:
    case AUDIO_FORMAT_DOLBY_TRUEHD:
    case AUDIO_FORMAT_EVRC:
    case AUDIO_FORMAT_EVRCB:
    case AUDIO_FORMAT_EVRCWB:
    case AUDIO_FORMAT_EVRCNW:
    case AUDIO_FORMAT_AAC_ADIF:
    case AUDIO_FORMAT_WMA:
    case AUDIO_FORMAT_WMA_PRO:
    case AUDIO_FORMAT_AMR_WB_PLUS:
    case AUDIO_FORMAT_MP2:
    case AUDIO_FORMAT_QCELP:
    case AUDIO_FORMAT_DSD:
    case AUDIO_FORMAT_FLAC:
    case AUDIO_FORMAT_ALAC:
    case AUDIO_FORMAT_APE:
        return true;
    case AUDIO_FORMAT_AAC_ADTS:
        switch (format) {
        case AUDIO_FORMAT_AAC_ADTS:
        case AUDIO_FORMAT_AAC_ADTS_MAIN:
        case AUDIO_FORMAT_AAC_ADTS_LC:
        case AUDIO_FORMAT_AAC_ADTS_SSR:
        case AUDIO_FORMAT_AAC_ADTS_LTP:
        case AUDIO_FORMAT_AAC_ADTS_HE_V1:
        case AUDIO_FORMAT_AAC_ADTS_SCALABLE:
        case AUDIO_FORMAT_AAC_ADTS_ERLC:
        case AUDIO_FORMAT_AAC_ADTS_LD:
        case AUDIO_FORMAT_AAC_ADTS_HE_V2:
        case AUDIO_FORMAT_AAC_ADTS_ELD:
        case AUDIO_FORMAT_AAC_ADTS_XHE:
            return true;
        default:
            return false;
        }
        /* not reached */
    case AUDIO_FORMAT_SBC:
    case AUDIO_FORMAT_APTX:
    case AUDIO_FORMAT_APTX_HD:
    case AUDIO_FORMAT_AC4:
    case AUDIO_FORMAT_LDAC:
        return true;
    case AUDIO_FORMAT_MAT:
        switch (format) {
        case AUDIO_FORMAT_MAT:
        case AUDIO_FORMAT_MAT_1_0:
        case AUDIO_FORMAT_MAT_2_0:
        case AUDIO_FORMAT_MAT_2_1:
            return true;
        default:
            return false;
        }
        /* not reached */
    case AUDIO_FORMAT_AAC_LATM:
        switch (format) {
        case AUDIO_FORMAT_AAC_LATM:
        case AUDIO_FORMAT_AAC_LATM_LC:
        case AUDIO_FORMAT_AAC_LATM_HE_V1:
        case AUDIO_FORMAT_AAC_LATM_HE_V2:
            return true;
        default:
            return false;
        }
        /* not reached */
    case AUDIO_FORMAT_CELT:
    case AUDIO_FORMAT_APTX_ADAPTIVE:
    case AUDIO_FORMAT_LHDC:
    case AUDIO_FORMAT_LHDC_LL:
    case AUDIO_FORMAT_APTX_TWSP:
    case AUDIO_FORMAT_LC3:
        return true;
    case AUDIO_FORMAT_MPEGH:
        switch (format) {
        case AUDIO_FORMAT_MPEGH_BL_L3:
        case AUDIO_FORMAT_MPEGH_BL_L4:
        case AUDIO_FORMAT_MPEGH_LC_L3:
        case AUDIO_FORMAT_MPEGH_LC_L4:
            return true;
        default:
            return false;
        }
        /* not reached */
    case AUDIO_FORMAT_DTS_UHD:
    case AUDIO_FORMAT_DRA:
        return true;
    default:
        return false;
    }
}
```

### 6.3.6 step 6

**初始化mChannelMask和mChannelCount**

```c++
    // 校验channelMask的输入
	if (!audio_is_output_channel(channelMask)) {
        ALOGE("%s(): Invalid channel mask %#x",  __func__, channelMask);
        status = BAD_VALUE;
        goto exit;
    }

    mChannelMask = channelMask;
	// 根据channelMask计算ChannelCount
    channelCount = audio_channel_count_from_out_mask(channelMask);
    mChannelCount = channelCount;
```

对channelMask的相关的两个函数定义在audio.h

```C++
static inline bool audio_is_output_channel(audio_channel_mask_t channel)
{
    uint32_t bits = audio_channel_mask_get_bits(channel);
    switch (audio_channel_mask_get_representation(channel)) {
    case AUDIO_CHANNEL_REPRESENTATION_POSITION:
        if (bits & ~AUDIO_CHANNEL_OUT_ALL) {
            bits = 0;
        }
        FALLTHROUGH_INTENDED;
    case AUDIO_CHANNEL_REPRESENTATION_INDEX:
        return bits != 0;
    default:
        return false;
    }
}

static inline uint32_t audio_channel_count_from_out_mask(audio_channel_mask_t channel)
{
    uint32_t bits = audio_channel_mask_get_bits(channel);
    switch (audio_channel_mask_get_representation(channel)) {
    case AUDIO_CHANNEL_REPRESENTATION_POSITION:
        // TODO: We can now merge with from_in_mask and remove anding
        bits &= AUDIO_CHANNEL_OUT_ALL;
        FALLTHROUGH_INTENDED;
    case AUDIO_CHANNEL_REPRESENTATION_INDEX:
        return __builtin_popcount(bits);
    default:
        return 0;
    }
}
```

### 6.3.7 step 7

**进一步调整flags**

step 4和step 5已经有针对flags的调整，此处主要设置了一些走AUDIO_OUTPUT_FLAG_DIRECT，也就是不需要混音，直接送到HAL的场景。

```c++
    // force direct flag if format is not linear PCM
    // or offload was requested
    if ((flags & AUDIO_OUTPUT_FLAG_COMPRESS_OFFLOAD)
            || !audio_is_linear_pcm(format)) {
        ALOGV( (flags & AUDIO_OUTPUT_FLAG_COMPRESS_OFFLOAD)
                    ? "%s(): Offload request, forcing to Direct Output"
                    : "%s(): Not linear PCM, forcing to Direct Output",
                    __func__);
        flags = (audio_output_flags_t)
                // FIXME why can't we allow direct AND fast?
                ((flags | AUDIO_OUTPUT_FLAG_DIRECT) & ~AUDIO_OUTPUT_FLAG_FAST);
    }

    // force direct flag if HW A/V sync requested
    if ((flags & AUDIO_OUTPUT_FLAG_HW_AV_SYNC) != 0) {
        flags = (audio_output_flags_t)(flags | AUDIO_OUTPUT_FLAG_DIRECT);
    }
```

### 6.3.8 step 8

**初始化mFrameSize**

```c++
    if (flags & AUDIO_OUTPUT_FLAG_DIRECT) {
        if (audio_has_proportional_frames(format)) {
            mFrameSize = channelCount * audio_bytes_per_sample(format);
        } else {
            mFrameSize = sizeof(uint8_t);
        }
    } else {
        ALOG_ASSERT(audio_has_proportional_frames(format));
        mFrameSize = channelCount * audio_bytes_per_sample(format);
        // createTrack will return an error if PCM format is not supported by server,
        // so no need to check for specific PCM formats here
    }
```

#### 6.3.8.1 mFrameSize

```c++
// mFrameSize定义了每个采样点的大小，单位：字节
// 一般情况下，等于 位宽 * 声道数
size_t mFrameSize;             // frame size in bytes
```

### 6.3.9 step 9

**初始化mSampleRate，mOriginalSampleRate，mPlaybackRate和mMaxRequiredSpeed**

```c++
    // step 9
    // sampling rate must be specified for direct outputs
	// OUTPUT_FLAG为DIRECT时，sampleRate不能为0
    if (sampleRate == 0 && (flags & AUDIO_OUTPUT_FLAG_DIRECT) != 0) {
        status = BAD_VALUE;
        goto exit;
    }

    mSampleRate = sampleRate;
	// 记录sampleRate的初始值
    mOriginalSampleRate = sampleRate;
    mPlaybackRate = AUDIO_PLAYBACK_RATE_DEFAULT;
    // 1.0 <= mMaxRequiredSpeed <= AUDIO_TIMESTRETCH_SPEED_MAX
    mMaxRequiredSpeed = min(max(maxRequiredSpeed, 1.0f), AUDIO_TIMESTRETCH_SPEED_MAX);

```

#### 6.3.9.1 mPlaybackRate

```c++
AudioPlaybackRate mPlaybackRate;
```

AudioPlaybackRate定义在

> frameworks/av/media/libaudioprocessing/include/media/AudioResamplerPublic.h
>
> libaudioprocessing.so

这个适用于混音的一个类，AudioMixer会用到，其定义如下：

```c++
using AudioPlaybackRate = ::audio_playback_rate_t;

struct audio_playback_rate {
    float mSpeed;
    float mPitch;
    audio_timestretch_stretch_mode_t  mStretchMode;
    audio_timestretch_fallback_mode_t mFallbackMode;
};

typedef struct audio_playback_rate audio_playback_rate_t;
```

对应了audio_playback_rate的结构体，可以看到该结构体中，有mSpeed（播放速度）、mPitch（音高）等属性。

> mPlaybackRate = AUDIO_PLAYBACK_RATE_DEFAULT;
>
> 再看下set()函数设置给mPlaybackRate的默认值：

```c++
static const AudioPlaybackRate AUDIO_PLAYBACK_RATE_DEFAULT = ::AUDIO_PLAYBACK_RATE_INITIALIZER;

static const audio_playback_rate_t AUDIO_PLAYBACK_RATE_INITIALIZER = {
    /* .mSpeed = */ AUDIO_TIMESTRETCH_SPEED_NORMAL,
    /* .mPitch = */ AUDIO_TIMESTRETCH_PITCH_NORMAL,
    /* .mStretchMode = */ AUDIO_TIMESTRETCH_STRETCH_DEFAULT,
    /* .mFallbackMode = */ AUDIO_TIMESTRETCH_FALLBACK_FAIL
};

#define AUDIO_TIMESTRETCH_SPEED_NORMAL 1.0f
#define AUDIO_TIMESTRETCH_PITCH_NORMAL 1.0f
```

可以看到默认的播放速度和音高都是1.0f

#### 6.3.9.2 mMaxRequiredSpeed

```c++
float mMaxRequiredSpeed;
```

先看下传参maxRequiredSpeed

> 1、AudioTrack.h中的默认值为1.0f
>
> 2、MediaPlayerService：1.0f
>
> 3、JNI中，没有传该值，采用缺省值1.0f

暂不清楚该成员的含义，应该表示最大支持的播放速度。set()中的赋值如下：

```c++
#define AUDIO_TIMESTRETCH_SPEED_MAX    20.0f
// 1.0 <= mMaxRequiredSpeed <= AUDIO_TIMESTRETCH_SPEED_MAX
    mMaxRequiredSpeed = min(max(maxRequiredSpeed, 1.0f), AUDIO_TIMESTRETCH_SPEED_MAX);
```

### 6.3.10 step 10

**初始化mOffloadInfoCopy、mOffloadInfo**

```c++
    // Make copy of input parameter offloadInfo so that in the future:
    //  (a) createTrack_l doesn't need it as an input parameter
    //  (b) we can support re-creation of offloaded tracks
    if (offloadInfo != NULL) {
        mOffloadInfoCopy = *offloadInfo;
        mOffloadInfo = &mOffloadInfoCopy;
    } else {
        mOffloadInfo = NULL;
        memset(&mOffloadInfoCopy, 0, sizeof(audio_offload_info_t));
    }
```

这两个成员的定义：

```c++
    audio_offload_info_t    mOffloadInfoCopy;
    const audio_offload_info_t* mOffloadInfo;
```

audio_offload_info_t是一个结构体，mOffloadInfoCopy是对mOffLoadInfo做的一次复制。

看下传参过来的offloadInfo：

> 1、AudioTrack.h中，默认NULL
>
> 2、MediaPlayerService中，如果有mCallback，offloadInfo非空，否则offloadInfo为空
>
> 3、JNI中，MODE_STATIC时offloadInfo为空，否则看情况

关于offloadInfo，后面专门研究，先跳过。

### 6.3.11 step 11

**初始化mVolume、mSendLevel和mReqFrameCount**

```c++
    mVolume[AUDIO_INTERLEAVE_LEFT] = 1.0f;
    mVolume[AUDIO_INTERLEAVE_RIGHT] = 1.0f;
    mSendLevel = 0.0f;
    // mFrameCount is initialized in createTrack_l
    mReqFrameCount = frameCount;
```

#### 6.3.11.1 mVolume

```c++
float mVolume[2];
```

mVolume记录左右声道的音量

#### 6.3.11.2 mSendLevel

```c++
float mSendLevel;
```

mSendLevel含义暂不明确

#### 6.3.11.3 mReqFrameCount

```c++
size_t mReqFrameCount; // frame count to request the first or next time
                       // a new IAudioTrack is needed, non-decreasing
```

先看下入参的frameCount：

> 1、AudioTrack.h中，缺省值为0
>
> 2、MediaPlayerService中，如果音频输出flag是AUDIO_OUTPUT_FLAG_COMPRESS_OFFLOAD（不走软解，走硬解），则frameCount为0；否则，计算一个frameCount的值
>
> 3、JNI中
>
> ```C++
>         // buffSizeInBytes是java层传过来的值，一般是通过getMInBufferSize()计算而来
> 		size_t frameCount;
>         if (audio_has_proportional_frames(format)) {
>             const size_t bytesPerSample = audio_bytes_per_sample(format);
>             frameCount = buffSizeInBytes / (channelCount * bytesPerSample);
>         } else {
>             frameCount = buffSizeInBytes;
>         }
> ```

综上，这个成员可以简单的理解为：缓冲区大小

### 6.3.12 step 12

**初始化mNotificationFramesReq、mNotificationsPerBufferReq和mNotificationFramesAct**

```c++
    if (notificationFrames >= 0) {
        mNotificationFramesReq = notificationFrames;
        mNotificationsPerBufferReq = 0;
    } else {
        if (!(flags & AUDIO_OUTPUT_FLAG_FAST)) {
            ALOGE("%s(): notificationFrames=%d not permitted for non-fast track",
                    __func__, notificationFrames);
            status = BAD_VALUE;
            goto exit;
        }
        if (frameCount > 0) {
            ALOGE("%s(): notificationFrames=%d not permitted with non-zero frameCount=%zu",
                    __func__, notificationFrames, frameCount);
            status = BAD_VALUE;
            goto exit;
        }
        mNotificationFramesReq = 0;
        const uint32_t minNotificationsPerBuffer = 1;
        const uint32_t maxNotificationsPerBuffer = 8;
        mNotificationsPerBufferReq = min(maxNotificationsPerBuffer,
                max((uint32_t) -notificationFrames, minNotificationsPerBuffer));
        ALOGW_IF(mNotificationsPerBufferReq != (uint32_t) -notificationFrames,
                "%s(): notificationFrames=%d clamped to the range -%u to -%u",
                __func__,
                notificationFrames, minNotificationsPerBuffer, maxNotificationsPerBuffer);
    }
    mNotificationFramesAct = 0;
```

看下这三个成员的定义

```c++
    // next 2 fields are const after constructor or set()
    // 执行完set()后，mNotificationFramesReq和mNotificationsPerBufferReq就是常量了
    
    // 每两次回调之间需要的音频帧数量
    uint32_t mNotificationFramesReq; // requested number of frames between each
                                                    // notification callback,
                                                    // at initial source sample rate
	// 每个buffer需要通知的数量
    uint32_t mNotificationsPerBufferReq;
                                                    // requested number of notifications per buffer,
                                                    // currently only used for fast tracks with
                                                    // default track buffer size
	// 每两次回调之间实际的音频帧数量
    uint32_t mNotificationFramesAct; // actual number of frames between each
                                                    // notification callback,
                                                    // at initial source sample rate
```

先看下入参notificaitonFrames

> 1、AudioTrack.h中缺省值为0
>
> 2、MediaPlayerService中：0
>
> 3、JNI中，也是0

### 6.3.13 step 13

**初始化mClientPid和mClientUid**

```c++
callingPid = IPCThreadState::self()->getCallingPid();
    myPid = getpid();
    if (uid == AUDIO_UID_INVALID || (callingPid != myPid)) {
        mClientUid = IPCThreadState::self()->getCallingUid();
    } else {
        mClientUid = uid;
    }
    if (pid == -1 || (callingPid != myPid)) {
        mClientPid = callingPid;
    } else {
        mClientPid = pid;
    }
```

先看下传参uid和pid

> 1、AudioTrack.h中，uid的缺省值为AUDIO_UID_INVALID（-1），pid的缺省值为-1
>
> 2、MediaPlayerService中，uid为mUid，pid为mPid，对应mediaserver进程和线程
>
> 3、JNI中，uid和pid都是-1
>
> 综上，只有在mediaserver进程调用的场景，uid和pid才不是-1

补充下注释：

```c++
callingPid = IPCThreadState::self()->getCallingPid();
    myPid = getpid();
    if (uid == AUDIO_UID_INVALID || (callingPid != myPid)) {
        // uid是-1时，代表是应用层调用的，通过通过getCallingUid获取uid
        mClientUid = IPCThreadState::self()->getCallingUid();
    } else {
        // uid不是-1时，代表是mediaserver进程调用的，直接用传参的uid
        mClientUid = uid;
    }
    if (pid == -1 || (callingPid != myPid)) {
        // 同理，pid为-1时，代表是应用层调用的，通过getCallingPid获取pid
        mClientPid = callingPid;
    } else {
        // pid不是-1时，代表是mediaserver进程调用的，直接用传参的pid
        mClientPid = pid;
    }
```

mClientUid和mClientPid分别代表调用方的uid和pid

### 6.3.14 step 14

**初始化mAuxEffected、mOrigFlags、mCbf**

```c++
    mAuxEffectId = 0;
    mOrigFlags = mFlags = flags;
    mCbf = cbf;
```

### 6.3.15 step 15

***启动AudioTrackThread***

```c++
    if (cbf != NULL) {
        mAudioTrackThread = new AudioTrackThread(*this);
        mAudioTrackThread->run("AudioTrack", ANDROID_PRIORITY_AUDIO, 0 /*stack*/);
        // thread begins in paused state, and will not reference us until start()
    }
```

这是AudioTrack非常重要的一个线程，这里不展开了，开专门的章节展开。

### 6.3.16 step 16

**创建IAudioTrack**

```c++
    // create the IAudioTrack
    {
        AutoMutex lock(mLock);
        status = createTrack_l();
    }

    if (status != NO_ERROR) {
        if (mAudioTrackThread != 0) {
            mAudioTrackThread->requestExit();   // see comment in AudioTrack.h
            mAudioTrackThread->requestExitAndWait();
            mAudioTrackThread.clear();
        }
        goto exit;
    }
```

这里同样也是AudioTrack创建过程非常重要的一步，也不展开了。

### 6.3.17 step 17

**其他成员的初始化**

```c++
    mUserData = user;
    mLoopCount = 0;
    mLoopStart = 0;
    mLoopEnd = 0;
    mLoopCountNotified = 0;
    mMarkerPosition = 0;
    mMarkerReached = false;
    mNewPosition = 0;
    mUpdatePeriod = 0;
    mPosition = 0;
    mReleased = 0;
    mStartNs = 0;
    mStartFromZeroUs = 0;
    AudioSystem::acquireAudioSessionId(mSessionId, mClientPid, mClientUid);
    mSequence = 1;
    mObservedSequence = mSequence;
    mInUnderrun = false;
    mPreviousTimestampValid = false;
    mTimestampStartupGlitchReported = false;
    mTimestampRetrogradePositionReported = false;
    mTimestampRetrogradeTimeReported = false;
    mTimestampStallReported = false;
    mTimestampStaleTimeReported = false;
    mPreviousLocation = ExtendedTimestamp::LOCATION_INVALID;
    mStartTs.mPosition = 0;
    mUnderrunCountOffset = 0;
    mFramesWritten = 0;
    mFramesWrittenServerOffset = 0;
    mFramesWrittenAtRestore = -1; // -1 is a unique initializer.
    mVolumeHandler = new media::VolumeHandler();
```

## 6.4 启动AudioTrackThread

展开看下6.3.15:

```c++
	if (cbf != NULL) {
        mAudioTrackThread = new AudioTrackThread(*this);
        mAudioTrackThread->run("AudioTrack", ANDROID_PRIORITY_AUDIO, 0 /*stack*/);
        // thread begins in paused state, and will not reference us until start()
    }
```

cbf不为NULL时，会创建AudioTrackThread，可见AudioTrackThread是需要cbf向java层进行回调的。

看下cbf的传参：

> 1、 在AudioTrack.h，cbf默认是NULL
>
> 2、 在MediaPlayerService，cbf可能为NULL，也可能不为NULL
>
> 3、 在JNI，cbf不为空

### 6.4.1 android native线程

了解AudioTrackThread之前，先了解下native层开发/启动一个Thread的方法

> 首先，创建一个类继承自Thread基类
>
> 然后，实现threadLoop()的纯虚函数
>
> 最后，创建该类的对象，并调用run()函数，启动线程

再看下run()函数：

```c++
/**
 * 启动一个线程，并执行器thread_loop()函数
 * @param name : 线程名
 * @param priority : 线程优先级
 * @param stack : TODO
 */
virtual status_t run(const char* name, int32_t priority = PRIORITY_DEFAULT, 
	size_t stack = 0);
```

### 6.4.2 AudioTrackThread

看下AudioTrackThread的构造函数：

```c++
// 看初始化列表，传入的receiver是AudioTrack对象，mPaused是true代表开始的时候是暂停状态
AudioTrack::AudioTrackThread::AudioTrackThread(AudioTrack& receiver)
    : Thread(true /* bCanCallJava */)  // binder recursion on restoreTrack_l() may call Java.
    , mReceiver(receiver), mPaused(true), mPausedInt(false), mPausedNs(0LL),
      mIgnoreNextPausedInt(false)
{
}
```

再看下AudioTrackThread的threadLoop()函数：

```c++
bool AudioTrack::AudioTrackThread::threadLoop()
{
    {
        AutoMutex _l(mMyLock);
		...
    }
	...

    // 核心代码
    nsecs_t ns = mReceiver.processAudioBuffer();

    ...
}
```

看下AudioTrack的processAudioBuffer():

```c++
nsecs_t AudioTrack::processAudioBuffer()
{
    // Currently the AudioTrack thread is not created if there are no callbacks.
    // Would it ever make sense to run the thread, even without callbacks?
    // If so, then replace this by checks at each use for mCbf != NULL.
    LOG_ALWAYS_FATAL_IF(mCblk == NULL);

    mLock.lock();
    if (mAwaitBoost) {
        mAwaitBoost = false;
        mLock.unlock();
        static const int32_t kMaxTries = 5;
        int32_t tryCounter = kMaxTries;
        uint32_t pollUs = 10000;
        do {
            int policy = sched_getscheduler(0) & ~SCHED_RESET_ON_FORK;
            if (policy == SCHED_FIFO || policy == SCHED_RR) {
                break;
            }
            usleep(pollUs);
            pollUs <<= 1;
        } while (tryCounter-- > 0);
        if (tryCounter < 0) {
            ALOGE("%s(%d): did not receive expected priority boost on time",
                    __func__, mPortId);
        }
        // Run again immediately
        return 0;
    }

    // Can only reference mCblk while locked
    int32_t flags = android_atomic_and(
        ~(CBLK_UNDERRUN | CBLK_LOOP_CYCLE | CBLK_LOOP_FINAL | CBLK_BUFFER_END), &mCblk->mFlags);

    // Check for track invalidation
    if (flags & CBLK_INVALID) {
        // for offloaded tracks restoreTrack_l() will just update the sequence and clear
        // AudioSystem cache. We should not exit here but after calling the callback so
        // that the upper layers can recreate the track
        if (!isOffloadedOrDirect_l() || (mSequence == mObservedSequence)) {
            status_t status __unused = restoreTrack_l("processAudioBuffer");
            // FIXME unused status
            // after restoration, continue below to make sure that the loop and buffer events
            // are notified because they have been cleared from mCblk->mFlags above.
        }
    }

    bool waitStreamEnd = mState == STATE_STOPPING;
    bool active = mState == STATE_ACTIVE;

    // Manage underrun callback, must be done under lock to avoid race with releaseBuffer()
    bool newUnderrun = false;
    if (flags & CBLK_UNDERRUN) {
#if 0
        // Currently in shared buffer mode, when the server reaches the end of buffer,
        // the track stays active in continuous underrun state.  It's up to the application
        // to pause or stop the track, or set the position to a new offset within buffer.
        // This was some experimental code to auto-pause on underrun.   Keeping it here
        // in "if 0" so we can re-visit this if we add a real sequencer for shared memory content.
        if (mTransfer == TRANSFER_SHARED) {
            mState = STATE_PAUSED;
            active = false;
        }
#endif
        if (!mInUnderrun) {
            mInUnderrun = true;
            newUnderrun = true;
        }
    }

    // Get current position of server
    Modulo<uint32_t> position(updateAndGetPosition_l());

    // Manage marker callback
    bool markerReached = false;
    Modulo<uint32_t> markerPosition(mMarkerPosition);
    // uses 32 bit wraparound for comparison with position.
    if (!mMarkerReached && markerPosition.value() > 0 && position >= markerPosition) {
        mMarkerReached = markerReached = true;
    }

    // Determine number of new position callback(s) that will be needed, while locked
    size_t newPosCount = 0;
    Modulo<uint32_t> newPosition(mNewPosition);
    uint32_t updatePeriod = mUpdatePeriod;
    // FIXME fails for wraparound, need 64 bits
    if (updatePeriod > 0 && position >= newPosition) {
        newPosCount = ((position - newPosition).value() / updatePeriod) + 1;
        mNewPosition += updatePeriod * newPosCount;
    }

    // Cache other fields that will be needed soon
    uint32_t sampleRate = mSampleRate;
    float speed = mPlaybackRate.mSpeed;
    const uint32_t notificationFrames = mNotificationFramesAct;
    if (mRefreshRemaining) {
        mRefreshRemaining = false;
        mRemainingFrames = notificationFrames;
        mRetryOnPartialBuffer = false;
    }
    size_t misalignment = mProxy->getMisalignment();
    uint32_t sequence = mSequence;
    sp<AudioTrackClientProxy> proxy = mProxy;

    // Determine the number of new loop callback(s) that will be needed, while locked.
    int loopCountNotifications = 0;
    uint32_t loopPeriod = 0; // time in frames for next EVENT_LOOP_END or EVENT_BUFFER_END

    if (mLoopCount > 0) {
        int loopCount;
        size_t bufferPosition;
        mStaticProxy->getBufferPositionAndLoopCount(&bufferPosition, &loopCount);
        loopPeriod = ((loopCount > 0) ? mLoopEnd : mFrameCount) - bufferPosition;
        loopCountNotifications = min(mLoopCountNotified - loopCount, kMaxLoopCountNotifications);
        mLoopCountNotified = loopCount; // discard any excess notifications
    } else if (mLoopCount < 0) {
        // FIXME: We're not accurate with notification count and position with infinite looping
        // since loopCount from server side will always return -1 (we could decrement it).
        size_t bufferPosition = mStaticProxy->getBufferPosition();
        loopCountNotifications = int((flags & (CBLK_LOOP_CYCLE | CBLK_LOOP_FINAL)) != 0);
        loopPeriod = mLoopEnd - bufferPosition;
    } else if (/* mLoopCount == 0 && */ mSharedBuffer != 0) {
        size_t bufferPosition = mStaticProxy->getBufferPosition();
        loopPeriod = mFrameCount - bufferPosition;
    }

    // These fields don't need to be cached, because they are assigned only by set():
    //     mTransfer, mCbf, mUserData, mFormat, mFrameSize, mFlags
    // mFlags is also assigned by createTrack_l(), but not the bit we care about.

    mLock.unlock();

    // get anchor time to account for callbacks.
    const nsecs_t timeBeforeCallbacks = systemTime();

    if (waitStreamEnd) {
        // FIXME:  Instead of blocking in proxy->waitStreamEndDone(), Callback thread
        // should wait on proxy futex and handle CBLK_STREAM_END_DONE within this function
        // (and make sure we don't callback for more data while we're stopping).
        // This helps with position, marker notifications, and track invalidation.
        struct timespec timeout;
        timeout.tv_sec = WAIT_STREAM_END_TIMEOUT_SEC;
        timeout.tv_nsec = 0;

        status_t status = proxy->waitStreamEndDone(&timeout);
        switch (status) {
        case NO_ERROR:
        case DEAD_OBJECT:
        case TIMED_OUT:
            if (status != DEAD_OBJECT) {
                // for DEAD_OBJECT, we do not send a EVENT_STREAM_END after stop();
                // instead, the application should handle the EVENT_NEW_IAUDIOTRACK.
                mCbf(EVENT_STREAM_END, mUserData, NULL);
            }
            {
                AutoMutex lock(mLock);
                // The previously assigned value of waitStreamEnd is no longer valid,
                // since the mutex has been unlocked and either the callback handler
                // or another thread could have re-started the AudioTrack during that time.
                waitStreamEnd = mState == STATE_STOPPING;
                if (waitStreamEnd) {
                    mState = STATE_STOPPED;
                    mReleased = 0;
                }
            }
            if (waitStreamEnd && status != DEAD_OBJECT) {
               return NS_INACTIVE;
            }
            break;
        }
        return 0;
    }

    // perform callbacks while unlocked
    if (newUnderrun) {
        mCbf(EVENT_UNDERRUN, mUserData, NULL);
    }
    while (loopCountNotifications > 0) {
        mCbf(EVENT_LOOP_END, mUserData, NULL);
        --loopCountNotifications;
    }
    if (flags & CBLK_BUFFER_END) {
        mCbf(EVENT_BUFFER_END, mUserData, NULL);
    }
    if (markerReached) {
        mCbf(EVENT_MARKER, mUserData, &markerPosition);
    }
    while (newPosCount > 0) {
        size_t temp = newPosition.value(); // FIXME size_t != uint32_t
        mCbf(EVENT_NEW_POS, mUserData, &temp);
        newPosition += updatePeriod;
        newPosCount--;
    }

    if (mObservedSequence != sequence) {
        mObservedSequence = sequence;
        mCbf(EVENT_NEW_IAUDIOTRACK, mUserData, NULL);
        // for offloaded tracks, just wait for the upper layers to recreate the track
        if (isOffloadedOrDirect()) {
            return NS_INACTIVE;
        }
    }

    // if inactive, then don't run me again until re-started
    if (!active) {
        return NS_INACTIVE;
    }

    // Compute the estimated time until the next timed event (position, markers, loops)
    // FIXME only for non-compressed audio
    uint32_t minFrames = ~0;
    if (!markerReached && position < markerPosition) {
        minFrames = (markerPosition - position).value();
    }
    if (loopPeriod > 0 && loopPeriod < minFrames) {
        // loopPeriod is already adjusted for actual position.
        minFrames = loopPeriod;
    }
    if (updatePeriod > 0) {
        minFrames = min(minFrames, (newPosition - position).value());
    }

    // If > 0, poll periodically to recover from a stuck server.  A good value is 2.
    static const uint32_t kPoll = 0;
    if (kPoll > 0 && mTransfer == TRANSFER_CALLBACK && kPoll * notificationFrames < minFrames) {
        minFrames = kPoll * notificationFrames;
    }

    // This "fudge factor" avoids soaking CPU, and compensates for late progress by server
    static const nsecs_t kWaitPeriodNs = WAIT_PERIOD_MS * 1000000LL;
    const nsecs_t timeAfterCallbacks = systemTime();

    // Convert frame units to time units
    nsecs_t ns = NS_WHENEVER;
    if (minFrames != (uint32_t) ~0) {
        // AudioFlinger consumption of client data may be irregular when coming out of device
        // standby since the kernel buffers require filling. This is throttled to no more than 2x
        // the expected rate in the MixerThread. Hence, we reduce the estimated time to wait by one
        // half (but no more than half a second) to improve callback accuracy during these temporary
        // data surges.
        const nsecs_t estimatedNs = framesToNanoseconds(minFrames, sampleRate, speed);
        constexpr nsecs_t maxThrottleCompensationNs = 500000000LL;
        ns = estimatedNs - min(estimatedNs / 2, maxThrottleCompensationNs) + kWaitPeriodNs;
        ns -= (timeAfterCallbacks - timeBeforeCallbacks);  // account for callback time
        // TODO: Should we warn if the callback time is too long?
        if (ns < 0) ns = 0;
    }

    // If not supplying data by EVENT_MORE_DATA or EVENT_CAN_WRITE_MORE_DATA, then we're done
    if (mTransfer != TRANSFER_CALLBACK && mTransfer != TRANSFER_SYNC_NOTIF_CALLBACK) {
        return ns;
    }

    // EVENT_MORE_DATA callback handling.
    // Timing for linear pcm audio data formats can be derived directly from the
    // buffer fill level.
    // Timing for compressed data is not directly available from the buffer fill level,
    // rather indirectly from waiting for blocking mode callbacks or waiting for obtain()
    // to return a certain fill level.

    struct timespec timeout;
    const struct timespec *requested = &ClientProxy::kForever;
    if (ns != NS_WHENEVER) {
        timeout.tv_sec = ns / 1000000000LL;
        timeout.tv_nsec = ns % 1000000000LL;
        ALOGV("%s(%d): timeout %ld.%03d",
                __func__, mPortId, timeout.tv_sec, (int) timeout.tv_nsec / 1000000);
        requested = &timeout;
    }

    size_t writtenFrames = 0;
    while (mRemainingFrames > 0) {

        Buffer audioBuffer;
        audioBuffer.frameCount = mRemainingFrames;
        size_t nonContig;
        status_t err = obtainBuffer(&audioBuffer, requested, NULL, &nonContig);
        LOG_ALWAYS_FATAL_IF((err != NO_ERROR) != (audioBuffer.frameCount == 0),
                "%s(%d): obtainBuffer() err=%d frameCount=%zu",
                 __func__, mPortId, err, audioBuffer.frameCount);
        requested = &ClientProxy::kNonBlocking;
        size_t avail = audioBuffer.frameCount + nonContig;
        ALOGV("%s(%d): obtainBuffer(%u) returned %zu = %zu + %zu err %d",
                __func__, mPortId, mRemainingFrames, avail, audioBuffer.frameCount, nonContig, err);
        if (err != NO_ERROR) {
            if (err == TIMED_OUT || err == WOULD_BLOCK || err == -EINTR ||
                    (isOffloaded() && (err == DEAD_OBJECT))) {
                // FIXME bug 25195759
                return 1000000;
            }
            ALOGE("%s(%d): Error %d obtaining an audio buffer, giving up.",
                    __func__, mPortId, err);
            return NS_NEVER;
        }

        if (mRetryOnPartialBuffer && audio_has_proportional_frames(mFormat)) {
            mRetryOnPartialBuffer = false;
            if (avail < mRemainingFrames) {
                if (ns > 0) { // account for obtain time
                    const nsecs_t timeNow = systemTime();
                    ns = max((nsecs_t)0, ns - (timeNow - timeAfterCallbacks));
                }

                // delayNs is first computed by the additional frames required in the buffer.
                nsecs_t delayNs = framesToNanoseconds(
                        mRemainingFrames - avail, sampleRate, speed);

                // afNs is the AudioFlinger mixer period in ns.
                const nsecs_t afNs = framesToNanoseconds(mAfFrameCount, mAfSampleRate, speed);

                // If the AudioTrack is double buffered based on the AudioFlinger mixer period,
                // we may have a race if we wait based on the number of frames desired.
                // This is a possible issue with resampling and AAudio.
                //
                // The granularity of audioflinger processing is one mixer period; if
                // our wait time is less than one mixer period, wait at most half the period.
                if (delayNs < afNs) {
                    delayNs = std::min(delayNs, afNs / 2);
                }

                // adjust our ns wait by delayNs.
                if (ns < 0 /* NS_WHENEVER */ || delayNs < ns) {
                    ns = delayNs;
                }
                return ns;
            }
        }

        size_t reqSize = audioBuffer.size;
        if (mTransfer == TRANSFER_SYNC_NOTIF_CALLBACK) {
            // when notifying client it can write more data, pass the total size that can be
            // written in the next write() call, since it's not passed through the callback
            audioBuffer.size += nonContig;
        }
        mCbf(mTransfer == TRANSFER_CALLBACK ? EVENT_MORE_DATA : EVENT_CAN_WRITE_MORE_DATA,
                mUserData, &audioBuffer);
        size_t writtenSize = audioBuffer.size;

        // Validate on returned size
        if (ssize_t(writtenSize) < 0 || writtenSize > reqSize) {
            ALOGE("%s(%d): EVENT_MORE_DATA requested %zu bytes but callback returned %zd bytes",
                    __func__, mPortId, reqSize, ssize_t(writtenSize));
            return NS_NEVER;
        }

        if (writtenSize == 0) {
            if (mTransfer == TRANSFER_SYNC_NOTIF_CALLBACK) {
                // The callback EVENT_CAN_WRITE_MORE_DATA was processed in the JNI of
                // android.media.AudioTrack. The JNI is not using the callback to provide data,
                // it only signals to the Java client that it can provide more data, which
                // this track is read to accept now.
                // The playback thread will be awaken at the next ::write()
                return NS_WHENEVER;
            }
            // The callback is done filling buffers
            // Keep this thread going to handle timed events and
            // still try to get more data in intervals of WAIT_PERIOD_MS
            // but don't just loop and block the CPU, so wait

            // mCbf(EVENT_MORE_DATA, ...) might either
            // (1) Block until it can fill the buffer, returning 0 size on EOS.
            // (2) Block until it can fill the buffer, returning 0 data (silence) on EOS.
            // (3) Return 0 size when no data is available, does not wait for more data.
            //
            // (1) and (2) occurs with AudioPlayer/AwesomePlayer; (3) occurs with NuPlayer.
            // We try to compute the wait time to avoid a tight sleep-wait cycle,
            // especially for case (3).
            //
            // The decision to support (1) and (2) affect the sizing of mRemainingFrames
            // and this loop; whereas for case (3) we could simply check once with the full
            // buffer size and skip the loop entirely.

            nsecs_t myns;
            if (audio_has_proportional_frames(mFormat)) {
                // time to wait based on buffer occupancy
                const nsecs_t datans = mRemainingFrames <= avail ? 0 :
                        framesToNanoseconds(mRemainingFrames - avail, sampleRate, speed);
                // audio flinger thread buffer size (TODO: adjust for fast tracks)
                // FIXME: use mAfFrameCountHAL instead of mAfFrameCount below for fast tracks.
                const nsecs_t afns = framesToNanoseconds(mAfFrameCount, mAfSampleRate, speed);
                // add a half the AudioFlinger buffer time to avoid soaking CPU if datans is 0.
                myns = datans + (afns / 2);
            } else {
                // FIXME: This could ping quite a bit if the buffer isn't full.
                // Note that when mState is stopping we waitStreamEnd, so it never gets here.
                myns = kWaitPeriodNs;
            }
            if (ns > 0) { // account for obtain and callback time
                const nsecs_t timeNow = systemTime();
                ns = max((nsecs_t)0, ns - (timeNow - timeAfterCallbacks));
            }
            if (ns < 0 /* NS_WHENEVER */ || myns < ns) {
                ns = myns;
            }
            return ns;
        }

        size_t releasedFrames = writtenSize / mFrameSize;
        audioBuffer.frameCount = releasedFrames;
        mRemainingFrames -= releasedFrames;
        if (misalignment >= releasedFrames) {
            misalignment -= releasedFrames;
        } else {
            misalignment = 0;
        }

        releaseBuffer(&audioBuffer);
        writtenFrames += releasedFrames;

        // FIXME here is where we would repeat EVENT_MORE_DATA again on same advanced buffer
        // if callback doesn't like to accept the full chunk
        if (writtenSize < reqSize) {
            continue;
        }

        // There could be enough non-contiguous frames available to satisfy the remaining request
        if (mRemainingFrames <= nonContig) {
            continue;
        }

#if 0
        // This heuristic tries to collapse a series of EVENT_MORE_DATA that would total to a
        // sum <= notificationFrames.  It replaces that series by at most two EVENT_MORE_DATA
        // that total to a sum == notificationFrames.
        if (0 < misalignment && misalignment <= mRemainingFrames) {
            mRemainingFrames = misalignment;
            return ((double)mRemainingFrames * 1100000000) / ((double)sampleRate * speed);
        }
#endif

    }
    if (writtenFrames > 0) {
        AutoMutex lock(mLock);
        mFramesWritten += writtenFrames;
    }
    mRemainingFrames = notificationFrames;
    mRetryOnPartialBuffer = true;

    // A lot has transpired since ns was calculated, so run again immediately and re-calculate
    return 0;
}
```

又是一个很长的代码，抽取下面部分：

```c++
    // perform callbacks while unlocked
    if (newUnderrun) {
        mCbf(EVENT_UNDERRUN, mUserData, NULL);
    }
    while (loopCountNotifications > 0) {
        mCbf(EVENT_LOOP_END, mUserData, NULL);
        --loopCountNotifications;
    }
    if (flags & CBLK_BUFFER_END) {
        mCbf(EVENT_BUFFER_END, mUserData, NULL);
    }
    if (markerReached) {
        mCbf(EVENT_MARKER, mUserData, &markerPosition);
    }
    while (newPosCount > 0) {
        size_t temp = newPosition.value(); // FIXME size_t != uint32_t
        mCbf(EVENT_NEW_POS, mUserData, &temp);
        newPosition += updatePeriod;
        newPosCount--;
    }

    if (mObservedSequence != sequence) {
        mObservedSequence = sequence;
        mCbf(EVENT_NEW_IAUDIOTRACK, mUserData, NULL);
        // for offloaded tracks, just wait for the upper layers to recreate the track
        if (isOffloadedOrDirect()) {
            return NS_INACTIVE;
        }
    }
```

可以粗略地知道AudioTrackThread是为了通过mCbf回调java层的。

## 6.4 createTrack_l()

重点看下这个函数：

```c++

status_t AudioTrack::createTrack_l()
{
    // step 1 : 获取IAudioFlinger，也就是BpAudioFlinger
    status_t status;
    bool callbackAdded = false;

    const sp<IAudioFlinger>& audioFlinger = AudioSystem::get_audio_flinger();
    if (audioFlinger == 0) {
        ALOGE("%s(%d): Could not get audioflinger",
                __func__, mPortId);
        status = NO_INIT;
        goto exit;
    }

    // step 2 : 
    {
    // mFlags (not mOrigFlags) is modified depending on whether fast request is accepted.
    // After fast request is denied, we will request again if IAudioTrack is re-created.
    // Client can only express a preference for FAST.  Server will perform additional tests.
    if (mFlags & AUDIO_OUTPUT_FLAG_FAST) {
        // either of these use cases:
        // use case 1: shared buffer
        bool sharedBuffer = mSharedBuffer != 0;
        bool transferAllowed =
            // use case 2: callback transfer mode
            (mTransfer == TRANSFER_CALLBACK) ||
            // use case 3: obtain/release mode
            (mTransfer == TRANSFER_OBTAIN) ||
            // use case 4: synchronous write
            ((mTransfer == TRANSFER_SYNC || mTransfer == TRANSFER_SYNC_NOTIF_CALLBACK)
                    && mThreadCanCallJava);

        bool fastAllowed = sharedBuffer || transferAllowed;
        if (!fastAllowed) {
            ALOGW("%s(%d): AUDIO_OUTPUT_FLAG_FAST denied by client,"
                  " not shared buffer and transfer = %s",
                  __func__, mPortId,
                  convertTransferToText(mTransfer));
            mFlags = (audio_output_flags_t) (mFlags & ~AUDIO_OUTPUT_FLAG_FAST);
        }
    }

    IAudioFlinger::CreateTrackInput input;
    if (mStreamType != AUDIO_STREAM_DEFAULT) {
        input.attr = AudioSystem::streamTypeToAttributes(mStreamType);
    } else {
        input.attr = mAttributes;
    }
    input.config = AUDIO_CONFIG_INITIALIZER;
    input.config.sample_rate = mSampleRate;
    input.config.channel_mask = mChannelMask;
    input.config.format = mFormat;
    input.config.offload_info = mOffloadInfoCopy;
    input.clientInfo.clientUid = mClientUid;
    input.clientInfo.clientPid = mClientPid;
    input.clientInfo.clientTid = -1;
    if (mFlags & AUDIO_OUTPUT_FLAG_FAST) {
        // It is currently meaningless to request SCHED_FIFO for a Java thread.  Even if the
        // application-level code follows all non-blocking design rules, the language runtime
        // doesn't also follow those rules, so the thread will not benefit overall.
        if (mAudioTrackThread != 0 && !mThreadCanCallJava) {
            input.clientInfo.clientTid = mAudioTrackThread->getTid();
        }
    }
    input.sharedBuffer = mSharedBuffer;
    input.notificationsPerBuffer = mNotificationsPerBufferReq;
    input.speed = 1.0;
    if (audio_has_proportional_frames(mFormat) && mSharedBuffer == 0 &&
            (mFlags & AUDIO_OUTPUT_FLAG_FAST) == 0) {
        input.speed  = !isPurePcmData_l() || isOffloadedOrDirect_l() ? 1.0f :
                        max(mMaxRequiredSpeed, mPlaybackRate.mSpeed);
    }
    input.flags = mFlags;
    input.frameCount = mReqFrameCount;
    input.notificationFrameCount = mNotificationFramesReq;
    input.selectedDeviceId = mSelectedDeviceId;
    input.sessionId = mSessionId;
    input.audioTrackCallback = mAudioTrackCallback;
    input.opPackageName = mOpPackageName;

    IAudioFlinger::CreateTrackOutput output;

    sp<IAudioTrack> track = audioFlinger->createTrack(input,
                                                      output,
                                                      &status);

    if (status != NO_ERROR || output.outputId == AUDIO_IO_HANDLE_NONE) {
        ALOGE("%s(%d): AudioFlinger could not create track, status: %d output %d",
                __func__, mPortId, status, output.outputId);
        if (status == NO_ERROR) {
            status = NO_INIT;
        }
        goto exit;
    }
    ALOG_ASSERT(track != 0);

    mFrameCount = output.frameCount;
    mNotificationFramesAct = (uint32_t)output.notificationFrameCount;
    mRoutedDeviceId = output.selectedDeviceId;
    mSessionId = output.sessionId;

    mSampleRate = output.sampleRate;
    if (mOriginalSampleRate == 0) {
        mOriginalSampleRate = mSampleRate;
    }

    mAfFrameCount = output.afFrameCount;
    mAfSampleRate = output.afSampleRate;
    mAfLatency = output.afLatencyMs;

    mLatency = mAfLatency + (1000LL * mFrameCount) / mSampleRate;

    // AudioFlinger now owns the reference to the I/O handle,
    // so we are no longer responsible for releasing it.

    // FIXME compare to AudioRecord
    sp<IMemory> iMem = track->getCblk();
    if (iMem == 0) {
        ALOGE("%s(%d): Could not get control block", __func__, mPortId);
        status = NO_INIT;
        goto exit;
    }
    // TODO: Using unsecurePointer() has some associated security pitfalls
    //       (see declaration for details).
    //       Either document why it is safe in this case or address the
    //       issue (e.g. by copying).
    void *iMemPointer = iMem->unsecurePointer();
    if (iMemPointer == NULL) {
        ALOGE("%s(%d): Could not get control block pointer", __func__, mPortId);
        status = NO_INIT;
        goto exit;
    }
    // invariant that mAudioTrack != 0 is true only after set() returns successfully
    if (mAudioTrack != 0) {
        IInterface::asBinder(mAudioTrack)->unlinkToDeath(mDeathNotifier, this);
        mDeathNotifier.clear();
    }
    mAudioTrack = track;
    mCblkMemory = iMem;
    IPCThreadState::self()->flushCommands();

    audio_track_cblk_t* cblk = static_cast<audio_track_cblk_t*>(iMemPointer);
    mCblk = cblk;

    mAwaitBoost = false;
    if (mFlags & AUDIO_OUTPUT_FLAG_FAST) {
        if (output.flags & AUDIO_OUTPUT_FLAG_FAST) {
            ALOGI("%s(%d): AUDIO_OUTPUT_FLAG_FAST successful; frameCount %zu -> %zu",
                  __func__, mPortId, mReqFrameCount, mFrameCount);
            if (!mThreadCanCallJava) {
                mAwaitBoost = true;
            }
        } else {
            ALOGD("%s(%d): AUDIO_OUTPUT_FLAG_FAST denied by server; frameCount %zu -> %zu",
                  __func__, mPortId, mReqFrameCount, mFrameCount);
        }
    }
    mFlags = output.flags;

    //mOutput != output includes the case where mOutput == AUDIO_IO_HANDLE_NONE for first creation
    if (mDeviceCallback != 0) {
        if (mOutput != AUDIO_IO_HANDLE_NONE) {
            AudioSystem::removeAudioDeviceCallback(this, mOutput, mPortId);
        }
        AudioSystem::addAudioDeviceCallback(this, output.outputId, output.portId);
        callbackAdded = true;
    }

    mPortId = output.portId;
    // We retain a copy of the I/O handle, but don't own the reference
    mOutput = output.outputId;
    mRefreshRemaining = true;

    // Starting address of buffers in shared memory.  If there is a shared buffer, buffers
    // is the value of pointer() for the shared buffer, otherwise buffers points
    // immediately after the control block.  This address is for the mapping within client
    // address space.  AudioFlinger::TrackBase::mBuffer is for the server address space.
    void* buffers;
    if (mSharedBuffer == 0) {
        buffers = cblk + 1;
    } else {
        // TODO: Using unsecurePointer() has some associated security pitfalls
        //       (see declaration for details).
        //       Either document why it is safe in this case or address the
        //       issue (e.g. by copying).
        buffers = mSharedBuffer->unsecurePointer();
        if (buffers == NULL) {
            ALOGE("%s(%d): Could not get buffer pointer", __func__, mPortId);
            status = NO_INIT;
            goto exit;
        }
    }

    mAudioTrack->attachAuxEffect(mAuxEffectId);

    // If IAudioTrack is re-created, don't let the requested frameCount
    // decrease.  This can confuse clients that cache frameCount().
    if (mFrameCount > mReqFrameCount) {
        mReqFrameCount = mFrameCount;
    }

    // reset server position to 0 as we have new cblk.
    mServer = 0;

    // update proxy
    if (mSharedBuffer == 0) {
        mStaticProxy.clear();
        mProxy = new AudioTrackClientProxy(cblk, buffers, mFrameCount, mFrameSize);
    } else {
        mStaticProxy = new StaticAudioTrackClientProxy(cblk, buffers, mFrameCount, mFrameSize);
        mProxy = mStaticProxy;
    }

    mProxy->setVolumeLR(gain_minifloat_pack(
            gain_from_float(mVolume[AUDIO_INTERLEAVE_LEFT]),
            gain_from_float(mVolume[AUDIO_INTERLEAVE_RIGHT])));

    mProxy->setSendLevel(mSendLevel);
    const uint32_t effectiveSampleRate = adjustSampleRate(mSampleRate, mPlaybackRate.mPitch);
    const float effectiveSpeed = adjustSpeed(mPlaybackRate.mSpeed, mPlaybackRate.mPitch);
    const float effectivePitch = adjustPitch(mPlaybackRate.mPitch);
    mProxy->setSampleRate(effectiveSampleRate);

    AudioPlaybackRate playbackRateTemp = mPlaybackRate;
    playbackRateTemp.mSpeed = effectiveSpeed;
    playbackRateTemp.mPitch = effectivePitch;
    mProxy->setPlaybackRate(playbackRateTemp);
    mProxy->setMinimum(mNotificationFramesAct);

    if (mDualMonoMode != AUDIO_DUAL_MONO_MODE_OFF) {
        setDualMonoMode_l(mDualMonoMode);
    }
    if (mAudioDescriptionMixLeveldB != -std::numeric_limits<float>::infinity()) {
        setAudioDescriptionMixLevel_l(mAudioDescriptionMixLeveldB);
    }

    mDeathNotifier = new DeathNotifier(this);
    IInterface::asBinder(mAudioTrack)->linkToDeath(mDeathNotifier, this);

    // This is the first log sent from the AudioTrack client.
    // The creation of the audio track by AudioFlinger (in the code above)
    // is the first log of the AudioTrack and must be present before
    // any AudioTrack client logs will be accepted.

    mMetricsId = std::string(AMEDIAMETRICS_KEY_PREFIX_AUDIO_TRACK) + std::to_string(mPortId);
    mediametrics::LogItem(mMetricsId)
        .set(AMEDIAMETRICS_PROP_EVENT, AMEDIAMETRICS_PROP_EVENT_VALUE_CREATE)
        // the following are immutable
        .set(AMEDIAMETRICS_PROP_FLAGS, toString(mFlags).c_str())
        .set(AMEDIAMETRICS_PROP_ORIGINALFLAGS, toString(mOrigFlags).c_str())
        .set(AMEDIAMETRICS_PROP_SESSIONID, (int32_t)mSessionId)
        .set(AMEDIAMETRICS_PROP_TRACKID, mPortId) // dup from key
        .set(AMEDIAMETRICS_PROP_CONTENTTYPE, toString(mAttributes.content_type).c_str())
        .set(AMEDIAMETRICS_PROP_USAGE, toString(mAttributes.usage).c_str())
        .set(AMEDIAMETRICS_PROP_THREADID, (int32_t)output.outputId)
        .set(AMEDIAMETRICS_PROP_SELECTEDDEVICEID, (int32_t)mSelectedDeviceId)
        .set(AMEDIAMETRICS_PROP_ROUTEDDEVICEID, (int32_t)mRoutedDeviceId)
        .set(AMEDIAMETRICS_PROP_ENCODING, toString(mFormat).c_str())
        .set(AMEDIAMETRICS_PROP_CHANNELMASK, (int32_t)mChannelMask)
        .set(AMEDIAMETRICS_PROP_FRAMECOUNT, (int32_t)mFrameCount)
        // the following are NOT immutable
        .set(AMEDIAMETRICS_PROP_VOLUME_LEFT, (double)mVolume[AUDIO_INTERLEAVE_LEFT])
        .set(AMEDIAMETRICS_PROP_VOLUME_RIGHT, (double)mVolume[AUDIO_INTERLEAVE_RIGHT])
        .set(AMEDIAMETRICS_PROP_STATE, stateToString(mState))
        .set(AMEDIAMETRICS_PROP_AUXEFFECTID, (int32_t)mAuxEffectId)
        .set(AMEDIAMETRICS_PROP_SAMPLERATE, (int32_t)mSampleRate)
        .set(AMEDIAMETRICS_PROP_PLAYBACK_SPEED, (double)mPlaybackRate.mSpeed)
        .set(AMEDIAMETRICS_PROP_PLAYBACK_PITCH, (double)mPlaybackRate.mPitch)
        .set(AMEDIAMETRICS_PROP_PREFIX_EFFECTIVE
                AMEDIAMETRICS_PROP_SAMPLERATE, (int32_t)effectiveSampleRate)
        .set(AMEDIAMETRICS_PROP_PREFIX_EFFECTIVE
                AMEDIAMETRICS_PROP_PLAYBACK_SPEED, (double)effectiveSpeed)
        .set(AMEDIAMETRICS_PROP_PREFIX_EFFECTIVE
                AMEDIAMETRICS_PROP_PLAYBACK_PITCH, (double)effectivePitch)
        .record();

    // mSendLevel
    // mReqFrameCount?
    // mNotificationFramesAct, mNotificationFramesReq, mNotificationsPerBufferReq
    // mLatency, mAfLatency, mAfFrameCount, mAfSampleRate

    }

exit:
    if (status != NO_ERROR && callbackAdded) {
        // note: mOutput is always valid is callbackAdded is true
        AudioSystem::removeAudioDeviceCallback(this, mOutput, mPortId);
    }

    mStatus = status;

    // sp<IAudioTrack> track destructor will cause releaseOutput() to be called by AudioFlinger
    return status;
}
```

### 6.4.1 step 1

**获取IAudioFlinger，也就是BpAudioFlinger**

```c++
    status_t status;
    bool callbackAdded = false;

    const sp<IAudioFlinger>& audioFlinger = AudioSystem::get_audio_flinger();
    if (audioFlinger == 0) {
        ALOGE("%s(%d): Could not get audioflinger",
                __func__, mPortId);
        status = NO_INIT;
        goto exit;
    }
```

直接看下AudioSystem::get_audio_flinger():

```c++
// establish binder interface to AudioFlinger service
const sp<IAudioFlinger> AudioSystem::get_audio_flinger()
{
    sp<IAudioFlinger> af;
    sp<AudioFlingerClient> afc;
    bool reportNoError = false;
    {
        Mutex::Autolock _l(gLock);
        if (gAudioFlinger == 0) {
            sp<IServiceManager> sm = defaultServiceManager();
            sp<IBinder> binder;
            do {
                binder = sm->getService(String16("media.audio_flinger"));
                if (binder != 0)
                    break;
                ALOGW("AudioFlinger not published, waiting...");
                usleep(500000); // 0.5 s
            } while (true);
            if (gAudioFlingerClient == NULL) {
                gAudioFlingerClient = new AudioFlingerClient();
            } else {
                reportNoError = true;
            }
            binder->linkToDeath(gAudioFlingerClient);
            gAudioFlinger = interface_cast<IAudioFlinger>(binder);
            LOG_ALWAYS_FATAL_IF(gAudioFlinger == 0);
            afc = gAudioFlingerClient;
            // Make sure callbacks can be received by gAudioFlingerClient
            ProcessState::self()->startThreadPool();
        }
        af = gAudioFlinger;
    }
    if (afc != 0) {
        int64_t token = IPCThreadState::self()->clearCallingIdentity();
        af->registerClient(afc);
        IPCThreadState::self()->restoreCallingIdentity(token);
    }
    if (reportNoError) reportError(NO_ERROR);
    return af;
}
```



# 7. offload模式


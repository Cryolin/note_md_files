# AudioFlinger

AudioFlinger是音频策略的执行者，负责输入输出流设备的管理及音频流数据的处理传输

## 1. 文件结构

AudioFlinger相关的代码位于

> frameworks/av/services/audioflinger

| 文件名             | 主要功能                                                     |
| ------------------ | ------------------------------------------------------------ |
| audioflinger.h     | 定义AudioFlinger类                                           |
| audioflinger.cpp   | 实现AudioFlinger类，IAudioFlinger对外暴露的接口一般在这里实现 |
| AudioResampler.cpp | 重采样处理类                                                 |
| AudioMixer.cpp     | 混音处理类                                                   |
| PlaybackTracks.h   | 定义Track                                                    |
| Tracks.cpp         | Track管理类，实现TrackHandle、Track（前者是后者的代理对象）  |
| Effects.cpp        | 音效处理类                                                   |
| Threads.h          | 定义AF中线程相关的类，例如PlaybackThread                     |
| Threads.cpp        | 回放线程和录制线程类。回放线程从FIFO中读取回放数据<br>并混音处理，然后写数据到输出流设备；录制线程从输入<br>流设备读取录音数据并重采样处理，然后写数据到FIFO |
|                    |                                                              |
|                    |                                                              |



## 2. AudioFlinger的执行逻辑



## 3. IAudioFlinger对外接口

| 函数名                      | 功能                                                         |
| --------------------------- | ------------------------------------------------------------ |
| sp<IAudioTrack> createTrack | 新建输出流管理对象： 找到对应的 PlaybackThread，创建输出流管理对象 Track，然后创建并返回该 Track 的代理对象 TrackHandle |
|                             |                                                              |
|                             |                                                              |



## 4. AudioFlinger的成员

| 成员                                                         | 含义 |
| ------------------------------------------------------------ | ---- |
| const sp<MediaLogNotifier> mMediaLogNotifier;                |      |
| AudioHwDevice* mPrimaryHardwareDev;                          |      |
| DefaultKeyedVector<audio_module_handle_t, AudioHwDevice*>  mAudioHwDevs; |      |
| mutable hardware_call_state mHardwareStatus;                 |      |
| float mMasterVolume;                                         |      |
| bool mMasterMute;                                            |      |
| audio_mode_t mMode;                                          |      |
| std::atomic_bool mBtNrecIsOff;                               |      |
| std::atomic<bool> mIsLowRamDevice;                           |      |
| bool mIsDeviceTypeKnown;                                     |      |
| int64_t mTotalMemory;                                        |      |
| std::atomic<size_t> mClientSharedHeapSize;                   |      |
| nsecs_t mGlobalEffectEnableTime;                             |      |
| PatchPanel mPatchPanel;                                      |      |
| DeviceEffectManager mDeviceEffectManager;                    |      |
| bool mSystemReady;                                           |      |

## 5. AudioFlinger的函数

| 函数                                        | 含义                 | 调用位置                                                     |
| ------------------------------------------- | -------------------- | ------------------------------------------------------------ |
| AudioFlinger::AudioFlinger()                | 构造函数             | audioserver进程启动时                                        |
| void AudioFlinger::onFirstRef()             |                      | audioserver进程启动时                                        |
| sp<IAudioTrack> AudioFlinger::createTrack() | 客户端创建AudioTrack | 客户端通过MediaPlayer.java、AudioTrack.java<br>等创建AudioTrack时，返回IAudioTrack给客户端 |
|                                             |                      |                                                              |

## 6. AudioFlinger的线程

![image-20211024165910465](.\images\image-20211024165910465.png)

### 6.1 线程执行时机

以PlaybackThread为例，在其作为sp<>第一次引用时，会启动该线程：

```c++
void AudioFlinger::PlaybackThread::onFirstRef()
{
    ...
    run(mThreadName, ANDROID_PRIORITY_URGENT_AUDIO);
}
```

### 6.2 回放线程

回放线程PlaybackThread主要用于音频的输出处理。在看回放线程之前，先回顾下AudioTrack中的一个重要的成员：

```c++
// AudioTrack.h
audio_output_flags_t mFlags;	
```

mFlags定义了该AudioTrack进行音频输出的方式，可能的值有：AUDIO_OUTPUT_FLAG_DIRECT等

在AudioTrack创建时，会调用AudioFlinger::createTrack()，将mFlags传给AudioFlinger，最终通过一系列流程（详见createTrack()的流程梳理）,根据不同的Flag值，选择不同的回放线程进行播放：最终的映射关系如下：

| 名称                              | 含义                                                         | 对应回放线程           |
| --------------------------------- | ------------------------------------------------------------ | ---------------------- |
| AUDIO_OUTPUT_FLAG_DIRECT          | 音频流不需要软件混音，直接交HAL处理，一般用于 HDMI 设备声音输出 | **DirectOutputThread** |
| AUDIO_OUTPUT_FLAG_PRIMARY         | 表示音频流需要输出到主输出设备，一般用于铃声类声音           | **MixerThread**        |
| AUDIO_OUTPUT_FLAG_FAST            | 表示音频流需要快速输出到音频设备，一般用于按键音、游戏背景音等对时延要求高的场景 | **MixerThread**        |
| AUDIO_OUTPUT_FLAG_DEEP_BUFFER     | 表示音频流输出可以接受较大的时延，一般用于音乐、视频播放等对时延要求不高的场景 | **MixerThread**        |
| AUDIO_OUTPUT_FLAG_COMPRES_OFFLOAD | 表示音频流没有经过软件解码，需要输出到硬件解码器，由硬件解码器进行解码 | **OffloadThread**      |

### 6.3 录音线程

## 7. AudioFlinger的启动

AudioFlinger服务的启动参考【音频框架-启动篇】

## 8. AudioFlinger中Track的创建

从本节开始，专注AudioFlinger中的代码走读。这里重点看下AudioFlinger::createTrack()


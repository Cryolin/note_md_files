# AudioPolicyService

## 1. 文件结构

## 2. AudioPolicyService相关结构体/枚举

### 2.1 设备类型：audio_devices_t

定义在audio-hal-enums.h，列举部分常见的（OUT输出设备，IN输入设备）：

| 名称                             | 值                         | 含义             |
| -------------------------------- | -------------------------- | ---------------- |
| 输出设备                         |                            |                  |
| AUDIO_DEVICE_OUT_EARPIECE        | 0x1                        | 听筒             |
| AUDIO_DEVICE_OUT_SPEAKER         | 0x2                        | 扬声器           |
| AUDIO_DEVICE_OUT_WIRED_HEADSET   | 0x4                        | 线控耳机         |
| AUDIO_DEVICE_OUT_WIRED_HEADPHONE | 0x8                        | 普通耳机         |
| AUDIO_DEVICE_OUT_BLUETOOTH_SCO   | 0x10                       | 蓝牙SCO          |
| AUDIO_DEVICE_OUT_REMOTE_SUBMIX   | 0x8000                     | RemoteSubmix输出 |
| AUDIO_DEVICE_OUT_PROXY           | 0x2000000                  | 虚拟设备         |
| 输入设备                         |                            |                  |
| AUDIO_DEVICE_IN_BUILTIN_MIC      | AUDIO_DEVICE_BIT_IN \|0x4u | 手机自带mic      |



### 2.2 audio_io_handle_t

定义在audio.h

```c++
/* AudioFlinger and AudioPolicy services use I/O handles to identify audio sources and sinks */
// AF和APS都是用这个值，去唯一性标识一个route
typedef int audio_io_handle_t;
```

看下这个值的初始化过程：

> 首先，APS启动的时候：
>
> AudioPolicyManager::onNewAudioModulesAvailableInt()

```c++
            sp<SwAudioOutputDescriptor> outputDesc = new SwAudioOutputDescriptor(outProfile,
                                                                                 mpClientInterface);
            audio_io_handle_t output = AUDIO_IO_HANDLE_NONE;
            status_t status = outputDesc->open(nullptr, DeviceVector(supportedDevice),
                                               AUDIO_STREAM_DEFAULT,
                                               AUDIO_OUTPUT_FLAG_NONE, &output);
```

SwAudioOutputDescriptor::open()

​	->AudioPolicyClient::openOutput()

​		-> AudioFlinger::openOutput()

> 最后，在AudioFlinger：

```c++
    if (*output == AUDIO_IO_HANDLE_NONE) {
        *output = nextUniqueId(AUDIO_UNIQUE_ID_USE_OUTPUT);
    } else {
        // Audio Policy does not currently request a specific output handle.
        // If this is ever needed, see openInput_l() for example code.
        ALOGE("openOutput_l requested output handle %d is not AUDIO_IO_HANDLE_NONE", *output);
        return 0;
    }
```

会根据当前最大的值，生成一个新的唯一的值。

// TODO

这里再详细说明一下 audio_io_handle_t，它是 AudioTrack/AudioRecord/AudioSystem、 AudioFlinger、AudioPolicyManager 之间一个重要的链结点。
AudioFlinger 回放录制线程 小节在 AudioFlinger::openOutput_l() 注释中大致说明了它的来历及其作用，现在回顾下：当打开输出流设备及创建 PlaybackThread 时，系统会分配一个全局唯一的值作为 audio_io_handle_t，并把 audio_io_handle_t 和 PlaybackThread 添加到键值对向量 mPlaybackThreads 中，由于 audio_io_handle_t 和 PlaybackThread 是一一对应的关系，因此拿到一个 audio_io_handle_t，就能遍历键值对向量 mPlaybackThreads 找到它对应的 PlaybackThread，可以简单理解 audio_io_handle_t 为 PlaybackThread 的索引号或线程 id。由于 audio_io_handle_t 具有 PlaybackThread 索引特性，所以应用进程想获取 PlaybackThread 某些信息的话，只需要传入对应的 audio_io_handle_t 即可。
例如 AudioFlinger::format(audio_io_handle_t output)，这是 AudioFlinger 的一个服务接口，用户进程可以通过该接口获取某个 PlaybackThread 配置的音频格式：

## 3. AudioPolicyService的成员

## 4. AudioPolicyService的函数

## 5. AudioPolicyService的启动


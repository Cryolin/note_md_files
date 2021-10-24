# 1. Android平台架构

## Q1：android架构

![image-20210505121208615](.\images\image-20210505121208615.png)

## Q2：Linux内核

Android平台的基础是Linux内核。例如Android Runtime（ART）依赖Linux内核来执行底层功能，例如线程和底层内存管理。

利用Linux内核可让Android利用主要安全功能，并且允许设备制造商为内核开发硬件驱动程序。

注意到android的binder是在Linux内核层的

## Q3：硬件抽象层（HAL）

HAL层，Hardware Abstraction Layer，是在具体的硬件平台上抽象出来的一个硬件接口层，北向向上层软件提供统一的接口，以实现上层软件与底层硬件的隔离。内部对南向的硬件的功能进行封装。

HAL层使得系统在新硬件上的移植变得异常简单，只需提供新硬件的抽象层即可。

## Q4：Android Runtime

Android的运行环境，从android5.0（api21）之后，从dalvik虚拟机切换至ART虚拟机。

ART虚拟机的特点：

1. AOT和JIT编译结合
2. 优化的垃圾回收（GC）
3. Android 9（api28）及更高版本的系统中，支持将应用软件包中的dex格式文件转换为更紧凑的机器代码

Android还包含一套核心运行时库，可提供Java API框架所使用的Java编程语言中的大部分功能，包括一些Java 8功能

## Q5：native C/C++ 库

许多核心Android系统组价和服务（例如ART和HAL）构建自原生代码，需要以C/C++编写的原生库。

例如开源的web浏览器引擎webkit，知名的libc库等。

## Q6：Java API 框架层

通过java语言实现，面向系统应用和普通应用提供开发接口。接口包括：

1. View系统
2. Resource系统
3. Notification系统
4. Activity Manager
5. Content Provider

## Q7：应用层

包括系统应用和其他普通应用，通过API框架层开发应用，无需关注底层实现。

# 2. AOSP环境配置

首先参考Linux.md中关于Linux安装与远程配置的步骤，完成配置。

## Q1：什么是repo？

repo是谷歌为有效管理安卓代码而开发的使用python语言、基于git命令和manifest-xml文件对多个git仓进行批量下载、提交等操作的工具工程。repo本身包含可执行入口脚本repo和工程脚本git仓，通过以repo开头的repo init、repo sync、repo upload等不同命令实现不同操作。

repo依赖的python版本是2.x，推荐2.7

# 3. Android编译

## 3.1 编译过程

编译过程包括：

1. 预处理：对各种预处理命令做处理，为编译阶段做准备
2. 编译：将预处理的代码编译成汇编代码
3. 汇编：把汇编代码转为机器代码（目标文件）
4. 链接：将多个目标文件以及所需的`库文件`链接生成可执行目标文件的过程

## 3.2 静态库&动态库

**`后缀`**

静态库：.a .lib

动态库：.so .dll

**`链接`**

静态库在链接阶段会被“拷贝”到最终的可执行文件中

动态库在链接阶段仅仅“拷贝”一些重定位和符号表信息

通过静态库链接的程序大小 > 通过动态库链接的程序大小

**`运行`**

静态库的空间浪费问题：

![image-20210822103345702](.\images\image-20210822103345702.png)

动态库实现一次加载，多进程使用

![image-20210822103410124](.\images\image-20210822103410124.png)

**`查看依赖的动态库`**

可以通过ldd命令，查看可执行文件依赖了哪些动态库，其路径如何：

```
$ ldd main
    linux-vdso.so.1 =>  (0x00007ffc7b5a2000)
    libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007fe9642bf000)
    libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007fe963ef5000)
    /lib64/ld-linux-x86-64.so.2 (0x00007fe9645c8000)
```

## 3.3 make&makefile

makefile是一个脚本，由make来解析。

（不局限于编译）makefile本身只是一种“规则”的执行者，而使用者具体通过它来做什么则没有任何限制。如既可以用它来架构编译系统，也能用来生成文档，或者单纯地打印log等。

目前软件行业有多款优秀的make解析程序，如GNU make(Android中采用的)，Visual Studio中的nmake等，尽管在表现形式上会有些差异，但“万变不离其宗”，它们都是通过以下基础规则拓展起来的：

> TARGET: PREREQIOSOTES
>
> ​        COMMANDS

> 每个COMMANDS前都必须有一个TAB制表符

详细可参考：https://www.bilibili.com/video/BV1iE41187R1?p=15

## 3.4 m&mm&mmm命令

这三个是android中编译模块的命令

要想使用这些命令，首先需要在android源码根目录执行. build/envsetup.sh 

```
m：编译所有的模块 
mm：编译当前目录下的模块（但是不包括它们的依赖），当前目录下要有Android.mk文件 
mmm：编译指定路径下的模块（但是不包括它们的依赖），指定路径下要有Android.mk文件 
```

## 3.5 Android.mk介绍

Android.mk是一个向Android NDK构建系统描述NDK项目的GNU makefile片段。主要用于编译生成以下几种：

- APK程序
- JAVA库
- C\C++应用程序
- C\C++静态库
- C\C++共享库

**一个简单的Android.mk文件**

![image-20210822141324424](.\images\image-20210822141324424.png)

**编译多个共享库**

![image-20210822141749553](.\images\image-20210822141749553.png)

**编译静态库**

![image-20210822142733776](.\images\image-20210822142733776.png)

**使用共享库（动态库）**

![image-20210822142910016](.\images\image-20210822142910016.png)

## 3.6 Android.mk开发示例

实际开发中，结合JNI和Android.mk实现动态库的构建、以及打包到APK中。详细的开发流程如下。

![image-20210822154709737](.\images\image-20210822154709737.png)

step 1：java层调用native方法：

```JAVA
// MainActivity.java
package com.colin.demo03;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {
    private TextView mTextView;

    static {
        System.loadLibrary("NDK-TEST");
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mTextView = findViewById(R.id.textView);
        mTextView.setText("" + native_test());
    }

    public native int native_test();
}
```

step 2：实现JNI方法

```C++
// ndk-test.c

#include <jni.h>

int ndk_test() {
    return 123;
}

// 注意ndk和test前面要加个“1”
#ifdef __cplusplus
extern "C" {
#endif
    jint Java_com_colin_demo03_MainActivity_native_1test(JNIEnv *env, jobject thiz) {
        return ndk_test();
    }
#ifdef __cplusplus
}
#endif

```

step 3：实现Android.mk

```makefile
# 定义模块当前路径
LOCAL_PATH := ${call my-dir}

# 清空环境变量
include ${CLEAR_VARS}

# 当前模块名
LOCAL_MODULE := ndk-test

# 当前模块包含的源代码文件
LOCAL_SRC_FILES := ndk_test.c

# 生成一个动态库
include $(BUILD_SHARED_LIBRARY)
```

step 4：修改build.gradle

```gradle
plugins {
    id 'com.android.application'
}

android {
    compileSdkVersion 30
    buildToolsVersion "30.0.3"

    defaultConfig {
        applicationId "com.colin.demo03"
        minSdkVersion 30
        targetSdkVersion 30
        versionCode 1
        versionName "1.0"

        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"

        externalNativeBuild {
            ndkBuild {
                abiFilters "x86"
            }
        }
    }

    externalNativeBuild {
        ndkBuild {
            path "src/main/jni/Android.mk"
        }
    }

	省略。。。
}
```

step 5：查看运行结果

![image-20210822155540908](.\images\image-20210822155540908.png)

## 3.7 Android.bp介绍

https://blog.csdn.net/tkwxty/article/details/104395820

https://note.qidong.name/demo/soong_build/

> cc_library_headers
>
> 对应Android.mk中的BUILD_HEADER_LIBRARY，头文件库

## 3.8 Android.mk和Android.bp映射关系

| Android.mk          | android.bp             | 说明                                         |
| ------------------- | ---------------------- | -------------------------------------------- |
| cc_binary           | BUILD_EXECUTABLE       | 编译成可执行文件                             |
| cc_library          | BUILD_SHARED_LIBRARY   | 默认编译成动态库                             |
| cc_library_shared   | BUILD_SHARED_LIBRARY   | 编译成动态库                                 |
| cc_library_headers  | BUILD_HEADER_LIBRARY   | 编译成头文件库                               |
| name                | LOCAL_MODULE           | 编译出模块的名称                             |
| srcs                | LOCAL_SRC_FILES        | 源文件                                       |
| include_dirs        | LOCAL_C_INCLUDES       | 头文件查找路径                               |
| export_include_dirs |                        | 外部依赖本库时，<br>添加到外部的include_dirs |
| cflags              | LOCAL_CFLAGS           |                                              |
| shared_libs         | LOCAL_SHARED_LIBRARIES | 依赖的动态库                                 |
| header_libs         | LOCAL_HEADER_LIBRARIES | 依赖的头文件库                               |
|                     |                        |                                              |
|                     |                        |                                              |



关于EXPORT相关的属性的含义，可以看：https://blog.csdn.net/huoxiaoqian123/article/details/115702372

## 3.9 Android系统编译过程

学习了上面关于makefile和Android.mk的知识后，看下Android系统的编译过程：

首先来到根目录，有个MAKEFILE文件：

```makefile
// 根目录/Makefile
### DO NOT EDIT THIS FILE ###
include build/make/core/main.mk
### DO NOT EDIT THIS FILE ###
```

build/make/core/main.mk文件特别长，建议结合《深入理解Android内核设计思想》第三章理解。

## 3.10 audioserver进程的编译

audioserver进程的mk文件位于

> frameworks\av\media\audioserver\Android.mk

根据上面的学习，对该文件注释如下：

```makefile
# 定义模块当前路径
LOCAL_PATH:= $(call my-dir)

# 清除除LOCAL_PATH外的环境变量
include $(CLEAR_VARS)

# 源文件列表
LOCAL_SRC_FILES := \
	main_audioserver.cpp \

# 依赖的动态库列表
LOCAL_SHARED_LIBRARIES := \
	libaaudioservice \
	libaudioflinger \
	libaudiopolicyservice \
	libaudioprocessing \
	libbinder \
	libcutils \
	liblog \
	libhidlbase \
	libmedia \
	libmedialogservice \
	libmediautils \
	libnbaio \
	libnblog \
	libutils \
	libvibrator

# 
LOCAL_HEADER_LIBRARIES := \
	libaudiohal_headers \
	libmediametrics_headers \

# 头文件路径
LOCAL_C_INCLUDES := \
	frameworks/av/services/audioflinger \
	frameworks/av/services/audiopolicy \
	frameworks/av/services/audiopolicy/common/managerdefinitions/include \
	frameworks/av/services/audiopolicy/common/include \
	frameworks/av/services/audiopolicy/engine/interface \
	frameworks/av/services/audiopolicy/service \
	frameworks/av/services/medialog \
	frameworks/av/services/oboeservice \
	frameworks/av/media/libaaudio/include \
	frameworks/av/media/libaaudio/src \
	frameworks/av/media/libaaudio/src/binding \
	frameworks/av/media/libmedia/include \
	external/sonic \

# 模块名
LOCAL_MODULE := audioserver
LOCAL_LICENSE_KINDS := SPDX-license-identifier-Apache-2.0
LOCAL_LICENSE_CONDITIONS := notice
LOCAL_NOTICE_FILE := $(LOCAL_PATH)/../../NOTICE

LOCAL_INIT_RC := audioserver.rc

LOCAL_CFLAGS := -Werror -Wall

# 生成可执行文件
include $(BUILD_EXECUTABLE)
```

看下audioserver进程入口类：main_audioserver.cpp

> frameworks/av/media/audioserver/main_audioserver.cpp

```C++
#include <binder/IPCThreadState.h>
#include <binder/ProcessState.h>
#include <binder/IServiceManager.h>
```

这几个类位于libbinder库中

> 思考:
>
> audioserver.exe的Android.mk位于frameworks\av\media\audioserver\Android.mk
>
> libbinder.so中的IPCThreadState.h等位于frameworks\native\libs\binder\include\binder
>
> 前者是如何拿到后者的include路径作为相对路径的？

要回答这个问题，先查看libbinder库的Android.bp文件（省略了部分内容）

>\frameworks\native\libs\binder\Android.bp

```makefile
省略。。。

cc_library_headers {		// 编译生成头文件库
    name: "libbinder_headers",		// 头文件库名称
    export_include_dirs: ["include"],	// 外部依赖本库时，把本库的include路径添加到外部的
    									// include_dirs中

	。。。

    header_libs: [			// 依赖的头文件库
        "libbase_headers",
        "libbinder_headers_platform_shared",
        "libcutils_headers",
        "libutils_headers",
    ],
    export_header_lib_headers: [	// 外部依赖本库时，把本库的如下头文件库添加到外部的
    								// header_libs中
        "libbase_headers",
        "libbinder_headers_platform_shared",
        "libcutils_headers",
        "libutils_headers",
    ],
	。。。
}

。。。

cc_library {			// 编译生成动态库
    name: "libbinder",		// 动态库名称

	。。。

    // 依赖的动态库
    include_dirs: [
        "bionic/libc/kernel/android/uapi/",
        "bionic/libc/kernel/uapi/",
    ],

	。。。

	// 源码
    srcs: [
        "Binder.cpp",
        "BpBinder.cpp",
        "BufferedTextOutput.cpp",
        "Debug.cpp",
        "IInterface.cpp",
        "IMemory.cpp",
        "IPCThreadState.cpp",
        "IResultReceiver.cpp",
        "IServiceManager.cpp",
        "IShellCallback.cpp",
        "LazyServiceRegistrar.cpp",
        "MemoryBase.cpp",
        "MemoryDealer.cpp",
        "MemoryHeapBase.cpp",
        "Parcel.cpp",
        "ParcelableHolder.cpp",
        "ParcelFileDescriptor.cpp",
        "PersistableBundle.cpp",
        "ProcessState.cpp",
        "RpcAddress.cpp",
        "RpcSession.cpp",
        "RpcServer.cpp",
        "RpcState.cpp",
        "Static.cpp",
        "Stability.cpp",
        "Status.cpp",
        "TextOutput.cpp",
        "Utils.cpp",
        ":packagemanager_aidl",
        ":libbinder_aidl",
    ],

	。。。

	// 依赖的动态库
    shared_libs: [
        "liblog",
        "libcutils",
        "libutils",
    ],

	// 依赖的头文件库
    header_libs: [
        "libbinder_headers",
    ],

	// 外部依赖本库时，将如下头文件库添加到外部的header_lib中
    export_header_lib_headers: [
        "libbinder_headers",
    ],
	
	。。。
}

省略。。。
```

可以看到，audioserver.exe依赖libbinder.so时，由于libbinder.so做了如下配置

```makefile
export_header_lib_headers: [
    "libbinder_headers",
],
```

所以audioserver.exe相当于依赖了libbinder_headers这个头文件库，而同样的，由于libbinder_headers做了如下配置

```makefile
export_include_dirs: ["include"]
```

所以audioserver.exe会把libbinder_headers的include路径添加到其include_dirs中，也就回答了前面的`思考`。

## 3.11 libbinder.so动态库的编译

接着audioserver进程的编译，继续看下libbinder.so动态库的编译。其Android.bp文件上节已经讨论过了。

同样的，看下其中一个类的include:

> frameworks\native\libs\binder\IServiceManager.cpp

```C++
#include <binder/IServiceManager.h>
#include <binder/IPCThreadState.h>
#include <binder/Parcel.h>

#include <android/os/BnServiceCallback.h>
#include <android/os/IServiceManager.h>
```

前三个比较好理解，参考上一节的分析，libbinder.so依赖了头文件库libbinder_headers，将"include"目录添加到了自己的include_dirs环境变量中。

关于后面两个，同样抛出该问题：

> 思考：IServiceManager.h和BnSrviceCallback.h这两个文件并不存在，这里是如何通过相对路径依赖进来的？
>
> TODO

# 4. 系统启动系列

## 4.1 启动流程

![android-boot](.\images\android-boot.jpg)

**图解：** Android系统启动过程由上图从下往上的一个过程是由Boot Loader引导开机，然后依次进入 -> `Kernel` -> `Native` -> `Framework` -> `App`，接来下简要说说每个过程：

Loader层：

- Boot ROM: 当手机处于关机状态时，长按Power键开机，引导芯片开始从固化在`ROM`里的预设代码开始执行，然后加载引导程序到`RAM`；
- Boot Loader：这是启动Android系统之前的引导程序，主要是检查RAM，初始化硬件参数等功能。

Kernel层：

- 启动Kernel的swapper进程(pid=0)：该进程又称为idle进程, 系统初始化过程Kernel由无到有开创的第一个进程, 用于初始化进程管理、内存管理，加载Display,Camera Driver，Binder Driver等相关工作；
- 启动kthreadd进程（pid=2）：是Linux系统的内核进程，会创建内核工作线程kworkder，软中断线程ksoftirqd，thermal等内核守护进程。`kthreadd进程是所有内核进程的鼻祖`。

native层：

​			启动init进程(pid=1),是Linux系统的用户进程，`init进程是所有用户进程的鼻祖`。

- init进程会孵化出ueventd、logd、healthd、installd、adbd、lmkd等用户守护进程；
- init进程还启动`servicemanager`(binder服务管家)、`bootanim`(开机动画)等重要服务
- init进程孵化出Zygote进程，Zygote进程是Android系统的第一个Java进程(即虚拟机进程)，`Zygote是所有Java进程的父进程`，Zygote进程本身是由init进程孵化而来的。

Framework层：

- Zygote进程，是由init进程通过解析init.rc文件后fork生成的，Zygote进程主要包含：
  - 加载ZygoteInit类，注册Zygote Socket服务端套接字
  - 加载虚拟机
  - 提前加载类preloadClasses
  - 提前加载资源preloadResources
- System Server进程，是由Zygote进程fork而来，`System Server是Zygote孵化的第一个进程`，System Server负责启动和管理整个Java framework，包含ActivityManager，WindowManager，PackageManager，PowerManager等服务。
- Media Server进程，是由init进程fork而来，负责启动和管理整个C++ framework，包含AudioFlinger，Camera Service等服务。

APP层：

- Zygote进程孵化出的第一个App进程是Launcher，这是用户看到的桌面App；
- Zygote进程还会创建Browser，Phone，Email等App进程，每个App至少运行在一个进程上。
- 所有的App进程都是由Zygote进程fork生成的。

Syscall && JNI

- Native与Kernel之间有一层系统调用(SysCall)层，见[Linux系统调用(Syscall)原理](http://gityuan.com/2016/05/21/syscall/);
- Java层与Native(C/C++)层之间的纽带JNI，见[Android JNI原理分析](http://gityuan.com/2016/05/28/android-jni/)。

## 4.2 核心进程汇总

![android-booting](.\images\android-booting.jpg)

| 序号 | 进程               | 入口类路径                                        | 主方法                | 概述                                                         |
| ---- | ------------------ | ------------------------------------------------- | --------------------- | ------------------------------------------------------------ |
| 1    | init进程           | /system/core/init/main.cpp                        | main.main()           | Linux系统中用户空间的第一个进程                              |
| 2    | zygote进程         | frameworks/base/cmds<br>/app_process/app_main.cpp | app_main.main()       | 所有Java进程的父进程                                         |
| 3    | system_server进程  |                                                   | SystemServer.main()   | 系统各大服务的载体                                           |
| 4    | app_process进程    |                                                   | RuntimeInit.main()    | 通过/system/bin/app_process启动的进程，且后面跟的参数不带–zygote，即并非启动zygote进程。 比如常见的有通过adb shell方式来执行am,pm等命令，便是这种方式。 |
| 5    | app进程            |                                                   | ActivityThread.main() | 通过Process.start启动App进程                                 |
| 6    | servicemanager进程 |                                                   |                       | binder服务的大管家, 守护进程循环运行在binder_loop            |

## 4.3 init进程

https://blog.csdn.net/yiranfeng/article/details/103549394

补充：

1. Android7.0后，init.rc进行了拆分，每个服务都有自己的rc文件，他们基本上都被加载到/system/etc/init，/vendor/etc/init, /odm/etc/init等目录，等init.rc解析完成后，会来解析这些目录中的rc文件，用来执行相关的动作。例如，音频相关的audioserver.rc位于/system/etc/init目录下。

## 4.4 servicemanager进程

## 4.5 zygote进程

https://blog.csdn.net/yiranfeng/article/details/103549872

![image-20210801201927790](.\images\image-20210801201927790.png)

补充：

1. zygote的rc文件中，配置了socket信息：同时，根据rc文件不同，可能启动多个zygote进程

   ```
   /system/core/rootdir/init.zygote64_32.rc
   
   // 启动zygote进程
   service zygote /system/bin/app_process64 -Xzygote /system/bin --zygote --start-system-server --socket-name=zygote
   	// 创建一个名为dev/socket/zygote，类型为stream，权限为660的socket
   	// 注意这里的/system/bin/app_process64是设备中可执行文件的路径，对应的源码并非这个位置
   	socket zygote stream 660 root system
   
   // 启动zygote_secondary进程
   service zygote_secondary /system/bin/app_process32 -Xzygote /system/bin --zygote --socket-name=zygote_secondary --enable-lazy-preload
   
   	// 创建一个名为dev/socket/zygote_secondary，类型为stream，权限为660的socket
   	socket zygote_secondary stream 660 root system
   ```
   
2. 关于AndroidRuntime::startReg(JNIEnv* env)函数

   ```C++
   // frameworks/base/core/jni/AndroidRuntime.cpp
   /*static*/ int AndroidRuntime::startReg(JNIEnv* env)
   {
       ATRACE_NAME("RegisterAndroidNatives");
       /*
        * This hook causes all future threads created in this process to be
        * attached to the JavaVM.  (This needs to go away in favor of JNI
        * Attach calls.)
        */
       androidSetCreateThreadFunc((android_create_thread_fn) javaCreateThreadEtc);
   
       ALOGV("--- registering native functions ---\n");
   
       /*
        * Every "register" function calls one or more things that return
        * a local reference (e.g. FindClass).  Because we haven't really
        * started the VM yet, they're all getting stored in the base frame
        * and never released.  Use Push/Pop to manage the storage.
        */
       env->PushLocalFrame(200);
   
       // 
       if (register_jni_procs(gRegJNI, NELEM(gRegJNI), env) < 0) {
           env->PopLocalFrame(NULL);
           return -1;
       }
       env->PopLocalFrame(NULL);
   
       //createJavaThread("fubar", quickTest, (void*) "hello");
   
       return 0;
   }
   ```

   register_jni_procs()函数相关的几个定义：

   ```C++
   // frameworks/base/core/jni/AndroidRuntime.cpp
   #ifdef NDEBUG
       #define REG_JNI(name)      { name }
       struct RegJNIRec {
           int (*mProc)(JNIEnv*);
       };
   #else
       #define REG_JNI(name)      { name, #name }
       struct RegJNIRec {
           int (*mProc)(JNIEnv*);
           const char* mName;
       };
   #endif
   
   typedef void (*RegJAMProc)();
   
   static int register_jni_procs(const RegJNIRec array[], size_t count, JNIEnv* env)
   {
       for (size_t i = 0; i < count; i++) {
           if (array[i].mProc(env) < 0) {
   #ifndef NDEBUG
               ALOGD("----------!!! %s failed to load\n", array[i].mName);
   #endif
               return -1;
           }
       }
       return 0;
   }
   
   static const RegJNIRec gRegJNI[] = {
           REG_JNI(register_com_android_internal_os_RuntimeInit),
           REG_JNI(register_com_android_internal_os_ZygoteInit_nativeZygoteInit),
           REG_JNI(register_android_os_SystemClock),
           REG_JNI(register_android_util_EventLog),
       ...
   }
   ```

   相当于依次执行gRegJNI数组中的函数指针中的函数，以register_com_android_internal_os_RuntimeInit为例：

   ```C++
   // frameworks/base/core/jni/AndroidRuntime.cpp
   int register_com_android_internal_os_RuntimeInit(JNIEnv* env)
   {
       // JNINativeMethod记录了java方法与native函数的映射关系
       // 关于JNI详细的说明见对应章节
       const JNINativeMethod methods[] = {
               {"nativeFinishInit", "()V",
                (void*)com_android_internal_os_RuntimeInit_nativeFinishInit},
               {"nativeSetExitWithoutCleanup", "(Z)V",
                (void*)com_android_internal_os_RuntimeInit_nativeSetExitWithoutCleanup},
       };
       
       // AndroidRuntime::jniRegisterNativeMethods实现JNI注册
       return jniRegisterNativeMethods(env, "com/android/internal/os/RuntimeInit",
           methods, NELEM(methods));
   }
   ```

3. 关于ZygoteInit.main()函数

   

## 4.6 systemserver进程

# 5. JNI&NDK

## 5.1 JNI开发的一般流程

step 1. 创建一个java文件，定义native方法并调用

```java
package com.colin.jni;

public class Main {
    public static void main(String[] args) {
        Main main = new Main();
        main.getPassWord();
    }

    public native String getPassWord();
}
```

step 2. 在java项目src路径下执行javap命令，创建jni的库文件

![image-20210808101431120](.\images\image-20210808101431120.png)

​	命令执行完后，可以看到src同级目录下，创建了jni目录，以及对应的库文件：

```C
// 文件名：com_colin_jni_Main.h
// 命名格式：包名_java文件名.h

/* DO NOT EDIT THIS FILE - it is machine generated */
#include <jni.h>
/* Header for class com_colin_jni_Main */

#ifndef _Included_com_colin_jni_Main
#define _Included_com_colin_jni_Main
#ifdef __cplusplus
extern "C" {
#endif
/*
 * Class:     com_colin_jni_Main
 * Method:    getPassWord
 * Signature: ()Ljava/lang/String;
 */
// JNIEXPORT 不能省略，代表此JNI方法可以被外部调用
// jstring 代表Java中的String
// JNICALL 可以省略，代表一次JNI调用
// Java_com_colin_jni_Main_getPassWord，JNI中调用的方法
// JNIEnv 非常重要的类，JNI中连接C/C++与java的桥梁
// jobject java传递下来的对象（如果是静态方法，传递jclass）
JNIEXPORT jstring JNICALL Java_com_colin_jni_Main_getPassWord
  (JNIEnv *, jobject);

#ifdef __cplusplus
}
#endif
#endif
```

step 3：把生成的库文件copy到C/C++项目中，简单修改下

![image-20210808102545298](.\images\image-20210808102545298.png)

step 4：去jdk安装目录，copy jni.h和jni_md.h到C/C++项目中

![image-20210808103212757](.\images\image-20210808103212757.png)

step 5：创建.c或者.cpp文件，实现jni函数

![image-20210808103451758](.\images\image-20210808103451758.png)

如果创建的是.c文件，这里的代码会有所不同，原因会在JNIEnv详解部分解释：

![image-20210808103729933](.\images\image-20210808103729933.png)

step 6：修改C/C++工程为动态库，生成该动态库

![image-20210808103915489](.\images\image-20210808103915489.png)

![image-20210808103948666](.\images\image-20210808103948666.png)

![image-20210808104043242](.\images\image-20210808104043242.png)

Step 7：java工程中，通过System.load()的方式加载该动态库

```java
package com.colin.jni;

public class Main {
    static {
        // System.loadLibrary()会从本项目libs路径下加载
        // System.load()会从其他路径下加载
        System.load("D:\\git\\C_plus_demo\\4. C++实用编程\\x64\\Debug\\4. C++实用编程.dll");
//        System.loadLibrary();
    }

    public static void main(String[] args) {
        Main main = new Main();
        System.out.println(main.getPassWord());
    }

    public native String getPassWord();
}
```

执行结果：

![image-20210808104429501](.\images\image-20210808104429501.png)

```
说明：这里演示的是在VS中构建动态库，Android工程中依赖动态库的方法，实际开发中，也可以通过Android.mk构建动态库，生成的动态库会自动打包到APK中，并位于libs目录下。详细参考【Android编译】章节
```

## 5.2 JNIEnv详解

JNIEnv是沟通C/C++与java之间的桥梁，java在loadLibrary或者registerJni的时候，会创建JNIEnv然后完成后续注册过程，将C/C++与java之间的函数映射建立起来。

在后续的native方法调用时，JNIEnv也会将调用传递给C/C++的jni方法，后者可以从JNIEnv中获取到java运行环境的对象、类以及其中的方法和成员状态，在jni方法执行完毕后，也会通过JNIEnv将执行结果回传到java环境。

回答上面的问题，为什么C和C++中对JNIEnv的调用方式不同？

查看JNIEnv的定义，位于jni.h

```C++
#ifdef __cplusplus
typedef JNIEnv_ JNIEnv;
#else
typedef const struct JNINativeInterface_ *JNIEnv;
#endif
```

*可以看到，对于C++，JNIEnv是结构体JNIEnv_的别名，是一级指针，对于C，JNIEnv是结构体JNINativeInterface_的指针别名，是二级指针，所以对应的调用方式不同*

## 5.3 android中的JNI

详见：http://gityuan.com/2016/05/28/android-jni/

## 5.4 JNI是如何注册的

无论是android系统启动时，在Zygote进程中startReg()开机注册，还是在具体的进程中调用System.loadLibrary()或System.load()注册，最终都是调用到JNIHelp.cpp的jniRegisterNativeMethods注册：

```C
libnativehelper\JNIHelp.cpp
int jniRegisterNativeMethods(JNIEnv* env, const char* className,
    const JNINativeMethod* methods, int numMethods)
{
    ALOGV("Registering %s's %d native methods...", className, numMethods);
    jclass clazz = (*env)->FindClass(env, className);
    ALOG_ALWAYS_FATAL_IF(clazz == NULL,
                         "Native registration unable to find class '%s'; aborting...",
                         className);
    int result = (*env)->RegisterNatives(env, clazz, methods, numMethods);
    (*env)->DeleteLocalRef(env, clazz);
    if (result == 0) {
        return 0;
    }
	
    ...
    return result;
}
```

上面的结构体JNINativeMethod定义如下：

```C
typedef struct {
    const char* name;  //Java层native函数名
    const char* signature; //Java函数签名，记录参数类型和个数，以及返回值类型
    void*       fnPtr; //Native层对应的函数指针
} JNINativeMethod;
```

JNINativeMethod定义了C/C++函数与java方法的映射，例如：

```C
static JNINativeMethod gMethods[] = {
    {"prepare",      "()V",  (void *)android_media_MediaPlayer_prepare},
    {"_start",       "()V",  (void *)android_media_MediaPlayer_start},
    {"_stop",        "()V",  (void *)android_media_MediaPlayer_stop},
    {"seekTo",       "(I)V", (void *)android_media_MediaPlayer_seekTo},
    {"_release",     "()V",  (void *)android_media_MediaPlayer_release},
    {"native_init",  "()V",  (void *)android_media_MediaPlayer_native_init},
    ...
};
```

详见：http://gityuan.com/2016/05/28/android-jni/

# 6. Binder

Binder内容比较多，单独开一个文件说明

## 6.1 什么是Binder？

A：Binder是android中主要的IPC方式，通过mmap实现一次拷贝，比Socket、管道传输速度更快，比共享内存更安全可控。

## 6.2 介绍下Linux的mmap函数的原理？它为何可用于实现Binder的一次拷贝？

相关linux概念：虚拟地址空间，物理内存等，详细参考【操作系统】

介绍mmap之前，需要了解很多概念

【概念1：页缓存】

**Linux进行I/O操作，将磁盘中的内容读取到Page Cache（页缓存），该层位于用户与磁盘之间，提升了对磁盘的访问性能**

**但是Page Cache是运行在内核空间的，用户空间不能直接寻址，所以当用户进程需使用该数据时，需将该数据从Page Cache拷贝到用户进程的堆内存**

【概念2：虚拟内存/虚拟地址空间】

**虚拟内存和虚拟地址空间是一个概念，下面统一用*虚拟地址空间***

**虚拟地址空间技术为每个进程提供了一个非常大的、一致的、私有的地址空间**

**它使得应用程序认为它拥有连续可用的内存（一个完整可用的地址空间），而实际上，它通常是被分割成多个物理内存碎片，还有部分暂时存储在外部磁盘储存器上，在需要时进行数据交换**

**对于32位系统，每个进程在创建加载的时候，都会被分配一个大小为4G（2^32）的连续的虚拟地址空间。虚拟的意思是，这个地址空间是不存在的，仅仅是每个进程“认为”自己拥有4G的内存，而实际上，它用了多少空间，操作系统就在磁盘上划出多少空间给它。**

**4G的虚拟地址空间，1G分配给内核空间，3G分配给用户空间**

**在进程真正运行的时候，需要某些数据并且数据不在物理内存中，才会触发*缺页异常*，进行数据拷贝**

【概念3：MMU】

**MMU（Memory Management Unit），内存管理单元，是一种负责处理CPU的内存访问请求的计算机硬件**

**负责虚拟地址与物理地址的转换，提供硬件机制的内存访问授权**

【概念4：从二次拷贝理解虚拟内存和物理内存】

**以将磁盘内容读取到用户空间为例，用户空间发起read()的系统调用，第一步将磁盘内容读取到内核空间的Page Cache，由于用户空间是无法直接寻址内核空间的，所以还需要第二步，将Page Cache的内容拷贝到用户空间。**

**整个操作过程，无论是用户态的操作，还是内核态的操作，MMU都屏蔽了具体的物理内存。**

**换句话说，第一步拷贝，把磁盘内容读取到内核态的虚拟地址空间（实际上，是读取到物理内存中，但MMU对CPU屏蔽了这些）。**

**第二步拷贝，把内核态的虚拟地址空间的内容，读取到用户态的虚拟地址空间（实际上，从物理内存的视角，是将内核态虚拟地址对应的物理内存中的内容，拷贝到用户态虚拟地址对应的物理内容中），但对于CPU来讲，这些操作是感知不到物理内存的**

**从上面的总结可以引出mmap，两次拷贝的模型中，用户态和内核态的虚拟地址空间，分别映射到不同的物理地址。假如可以让二者映射到同一块物理地址，是否就可以避免二次拷贝，使二者去同一块物理内存寻址呢？**

下面正式介绍mmap

**mmap是Linux提供的一种内存映射的方法，它将一个文件（或物理内存）映射到进程的地址空间中（既可以映射到用户地址空间，也可以映射到内核地址空间），实现文件磁盘地址和进程虚拟地址空间中一段虚拟地址的对应关系**

**mmap可以用于Binder，通过将Server进程的用户空间和内核空间的某一段虚拟地址映射到同一块物理内存，实现IPC通信的一次拷贝**

## 6.3 谈一谈Binder实现一次拷贝的流程？

**mmap是Linux提供的一种内存映射的方法，它将一个文件（或物理内存）映射到进程的地址空间中（既可以映射到用户地址空间，也可以映射到内核地址空间），实现文件磁盘地址和进程虚拟地址空间中一段虚拟地址的对应关系**

**mmap可以用于Binder，通过将Server进程的用户空间和内核空间的某一段虚拟地址映射到同一块物理内存，实现IPC通信的一次拷贝**

**其实现一次拷贝的流程如下：**

# 7. HIDL

在了解HIDL之前，先看几个概念：

## 7.1 OEM/ODM

https://zhuanlan.zhihu.com/p/40483304

**OEM(Original Equipment Manufacturer)**：原始设备制造商：我有研发能力，也有设计能力，就是懒得自己去生产。

**ODM(Original Design Manufacturer)**：原始设计制造商：我有渠道，我有idea，但我没研发能力，我提需求你帮我做出来吧。或者我有研发能力，可这东西太低端了，不值得我亲手去做，外包给你做吧。

## 7.2 分区

https://zhuanlan.zhihu.com/p/364003927

system分区：android系统的可执行程序、库、系统服务和app等。

vendor分区：厂商私有的可执行程序、库、系统服务和app等。

对于小米、华为等设备制造商，由于【使用的硬件/芯片】不同，所以会有自己针对不同芯片、不同硬件的HAL层的定制，这些定制都是位于vendor分区的。

## 7.3 O版本前的版本升级

在android O版本之前，每次android大版本升级，对于芯片供应商、硬件制造商和手机制造商来讲，都是一个庞大的工作量。O版本前的升级模型是这样的：

![image-20211024155234898](.\images\image-20211024155234898.png)

下面灰色的部分，可以理解为【芯片供应商、硬件供应商】等提供的vendor部件，也可以理解为HAL层，在O版本前，是以so的形式提供给【设备制造商的】。而设备制造商要做的事情，是把绿色的部分升级到新的android版本，然后把里面的接口与【芯片供应商、硬件供应商】提供的新的so进行联调。

O版本前，framework和HAL是在同一个进程的，framework通过dlopen()打开HAL的一个个so库，进行调用。



![image-20211024155506699](.\images\image-20211024155506699.png)

## 7.3 Treble

https://source.android.google.cn/devices/architecture/treble

Treble是google提出的用于简化android版本升级的框架。它利用HIDL技术，将framework和HAL分离到不同的进程，进程间通过HIDL进行IPC。而之所以做到了简化，是因为Treble提供了一套【稳定的】接口。

这样一来，【芯片供应商、硬件供应商】就不必链接system分区的每个大版本的接口变更，而疲于进行接口适配，简化了android的升级。

![image-20211024155901315](.\images\image-20211024155901315.png)

![image-20211024155917362](.\images\image-20211024155917362.png)

## 7.4 HIDL

HIDL是用于Android Framework与HAL进行IPC的接口。
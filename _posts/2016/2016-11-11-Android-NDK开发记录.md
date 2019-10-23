---
layout: post
title: Android-NDK开发记录
categories: Android
original: true
description: Android-NDK开发记录
keywords: Android，NDK
typora-root-url: ..\..
---

最近要在android中使用ffmpeg，因此涉及到NDK开发，之前没有用过，在此把相关使用做个记录，方便用时查看。

通过上网查看，发现现在NDK构建开发有两种方式：

1. 编写Application.mk 和 Android.mk 使用ndk-build命令来构建
2. 使用CMake工具来构建(Android studio 2.2以上才支持)

在此对这两种都做个记录，算是个总结吧。

## NDK-BUILD

Android.mk文件用来描述代码内部组织结构。
 
Application.mk文件用来告诉Android应用如何编译和使用这个本地库。

### Application.mk

在sdk\ndk-bundle\build\core的add-application.mk(其它.mk脚本中也定义了部分)中定义了很多默认的变量，读者可以自己去查看，下面列举几个作为解释：

APP_PROJECT_PATH

    这个变量是强制性的，并且会给出应用程序工程的根目录的一个绝对路径。

APP_MODULES

    这个变量是可选的，用来指定要使用的模块，对应Android.mk中的LOCAL_MODULE变量，如果没有定义，
	NDK将由在Android.mk中声明的模块编译，包含所有的makefile文件。
	如果APP_MODULES定义了，那它必须是一个空格分隔的模块列表，每个模块名字被定义在Android.mk文件中的LOCAL_MODULE中。


APP_OPTIM

    这个变量是可选的，用来定义“release”或"debug"。


APP_CFLAGS

    当编译模块中有C/C++文件的时候，在此可以更改C/c++编译器的参数，这样就不用直接更改Android.mk文件


APP_CXXFLAGS

    APP_CPPFLAGS的别名。

APP_CPPFLAGS

    当只有C++源文件的时候，可以修改这个C++编译器的参数。


APP_BUILD_SCRIPT

    默认情况下，NDK编译系统会在$(APP_PROJECT_PATH)/jni目录下寻找名为Android.mk文件($(APP_PROJECT_PATH)/jni/Android.mk)，
	如果你想覆盖此行为，你可以定义APP_BUILD_SCRIPT来指定一个备用的编译脚本。一个非绝对路径总是被解释为相对于NDK的顶层的目录。

APP_ABI

    默认情况下，NDK的编译系统使用"armeabi"ABI生成机器代码，为了兼容，用户可以修改这个变量以适应更多CPU架构。
	例如：APP_ABI := armeabi armeabi-v7a x86

APP_STL

	默认情况下，NDK的编译系统为最小的C++运行时库（/system/lib/libstdc++.so）提供C++头文件。
	然而，NDK的C++的实现，可以链接你自己需要的库在自己的应用程序中。
	例如：
	APP_STL := stlport_static    --> static STLport library
	APP_STL := stlport_shared    --> shared STLport library
	APP_STL := system            --> default C++ runtime library


### Android.mk

Android.mk语法就是GNU/makefile脚本，用来向编译器解释如何构建你的本地库，不过在这里使用也有几点规则需要约束。

	LOCAL_PATH := $(call my-dir)
	include $(CLEAR_VARS)
	
	LOCAL_MODULE := hello-jni
	LOCAL_SRC_FILES := hello-jni.c
	
	include $(BUILD_SHARED_LIBRARY)

如上就是一个简单的hello库描述，下面我们列举下每个变量或命令的意思：

LOCAL_PATH：=$(call my-dir)

    Android.mk文件必须以LOCAL_PATH变量开始，它用于在树中定位文件。在这个例子中，宏功能'my-dir'是由build system提供的，
	用于返回当前目录路径（包括Android.mk文件本身）

include $(CLEAR_VARS)

    CLEAR_VARS变量是由build system提供的，并且指明了一个GNU makefile文件，这个功能会清理掉所有以LOCAL_开头的内容（例如LOCAL_MODULE,LOCAL_SRC_FILES,LOCAL_STATIC_LIBRARIES等），
	除了LOCAL_PATH，这句话是必须的，因为如果所有的变量都是全局变量的话，所有的可控的编译文件都需要在一个单独的GNU中被解析并执行

LOCAL_MODULE :=hello-jni

    LOCAL_MODULE变量必须被定义，用来区分Android.mk中的每一个模块。文件名必须是唯一的，不能有空格。
	注意，这里编译器会为你自动加上一些前缀和后缀，来保证文件是一致的，比如：这里表明一个动态连接库模块被命名为"hello-jni"，
	但是最后会生成为"libhello-jni.so"文件。但是在Java中装载这个库的时候还是使用"hello-jni"名称。


LOCAL_SRC_FILES := hello-jni.c

    LOCAL_SRC_FILES变量被需包括一个C和C++源文件的列表，这些会编译并聚合到一个模块中。

include $(BUILD_SHARED_LIBRARY)

    执行构建命令，其中BUILD_SHARED_LIBRARY这个变量是由系统提供的，并且指定给GNU Makefile的脚本，
	它可以收集所有你定义的"include $(CLEAR_VARS)"中以LOCAL_开头的变量，并且决定哪些要被编译，
	哪些应该做的更加准确。编译生成的是以"lib<module_name>.so"的文件，这个就是共享库了。
	我们同样也可以使用BUILD_STATIC_LIBRARY编译系统便会生成一个以"lib<module_name>.a"的文件来供动态库调用。


另外需要注意的是有一些保留字段不能拿来定义普通的变量，比如：

1. 以LOCAL_开头的名称（如：LOCAL_MODULE）
2. 以PRIVATE_、NDK_、或APP_（内部使用）开始的名称
3. 小写的名称（例如'my-dir'，内部使用）

因为这些都是以ndk编译工具里已经定义的了，使用就会冲突，一般我们在定义自己的变量时都在之前加个“MY_”前缀，以方便区分。

和Application.mk一样，Android.mk也有很多定义好的变量可以使用，在此不作详细说明，读者可以自行去查看sdk\ndk-bundle\build\core里的definitions.mk文件。

## CMAKE-BUILD

在Android stuido 2.2以上，就可以使用CMake来管理C/C++代码了，使用更方便简洁，笔者习惯了python或gradle脚本，不喜欢makefile构建语法(曾经被=号两边的空格坑的好惨),因此ffmpeg就采用CMake来管理。

使用方法可以参考作者之前的一篇[博文](http://liuweiqiang.win/cmake/2016/06/03/Cmake-%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.html)

### 实例

以下是Android使用FFmpeg的构建脚本：

	cmake_minimum_required(VERSION 3.4.1)
	
	find_library( log-lib
	              log )
	
	set(CPP_DIR ${CMAKE_SOURCE_DIR})
	set(FFMPEG_LIB ${CPP_DIR}/ffmpeg/${ANDROID_ABI})
	
	add_library( avutil
	             STATIC
	             IMPORTED )
	set_target_properties( avutil
	                       PROPERTIES IMPORTED_LOCATION
	                       ${FFMPEG_LIB}/lib/libavutil.a )
	
	add_library( swresample
	             STATIC
	             IMPORTED )
	set_target_properties( swresample
	                       PROPERTIES IMPORTED_LOCATION
	                       ${FFMPEG_LIB}/lib/libswresample.a )
	add_library( avcodec
	             STATIC
	             IMPORTED )
	set_target_properties( avcodec
	                       PROPERTIES IMPORTED_LOCATION
	                       ${FFMPEG_LIB}/lib/libavcodec.a )
	add_library( avfilter
	             STATIC
	             IMPORTED)
	set_target_properties( avfilter
	                       PROPERTIES IMPORTED_LOCATION
	                       ${FFMPEG_LIB}/lib/libavfilter.a )
	add_library( swscale
	             STATIC
	             IMPORTED)
	set_target_properties( swscale
	                       PROPERTIES IMPORTED_LOCATION
	                       ${FFMPEG_LIB}/lib/libswscale.a )
	add_library( avdevice
	             STATIC
	             IMPORTED)
	set_target_properties( avdevice
	                       PROPERTIES IMPORTED_LOCATION
	                       ${FFMPEG_LIB}/lib/libavdevice.a )
	add_library( avformat
	             STATIC
	             IMPORTED)
	set_target_properties( avformat
	                       PROPERTIES IMPORTED_LOCATION
	                       ${FFMPEG_LIB}/lib/libavformat.a )
	
	include_directories(${FFMPEG_LIB}/include)

	include_directories(${CPP_DIR})
	
	AUX_SOURCE_DIRECTORY(${CPP_DIR} SRC_LIST)
	
	add_library( ffaudio
		         SHARED
	             ${SRC_LIST})

	target_link_libraries( ffaudio avformat avcodec swresample avfilter swscale avdevice avutil ${log-lib} z )


## JNI使用

构建配置搞好后，笔者熟悉C/C++，因此开发就简单了，唯一的问题就是要熟悉JNI规则。

### JNI关键点

JNI全程就是java native interface，因此它其实类似一个协议，JAVA和C/C++都互相遵守就能通信。

JNI函数表的组成就像C++的虚函数表，虚拟机可以运行多张函数表。

JNI接口指针仅在当前线程中起作用，指针不能从一个线程进入另一个线程，但可以在不同的线程中调用本地方法。

JNI的数据类型基本上都是JAVA类型前加个j，比如int -> jint。

JNI_OnLoad()与JNI_OnUnload()，当Android的VM(Virtual Machine)执行到System.loadLibrary()函数时，首先会去执行C组件里的JNI_OnLoad()函数。

其它的读者自行参看jni.h头文件就可以获取api的使用信息。需要注意的是内存释放的问题，如注意使用DeleteLocalRef来释放传给JAVA层的String。

### 查看java方法签名

在FFAudioDemo\app\build\intermediates\classes\debug目录打开命令行窗口，执行如下命令：

    javah  -classpath . -jni com.joinee.ffaudiodemo.ffaudio.FFAudio

### 查看二进制文件符号表

可以使用objdump来反编译.so文件，用来查看符号表，如：

arm-linux-androideabi-objdump.exe -t libffaudio.so > table.txt
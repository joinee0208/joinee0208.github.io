---
layout: post
title: Android app最大内存限制
categories: Android
original: true
description: Android app最大内存限制
keywords: Android
typora-root-url: ..\..
---

[1]:/images/android/2.png
[2]:/images/android/1.png


## 起因

最近开发了一款APP，平时内存占用保持在40M左右，今天心血来潮，想看看APP最大内存可以申请到多少，在此做了一个实验，实验环境：

- 小米3手机
- Android 4.4.4版本
- Android Studio 2.1.3 版本

## 实验代码

    Handler mHandler = new Handler();
    private List<byte[]> mlist = new ArrayList<>();
    private class test implements Runnable{
        @Override
        public void run() {

            mlist.add(new byte[1*1024*1024]);

            mHandler.postDelayed(new test(),100);

        }
    }

在app的启动后调用 `mHandler.postDelayed(new test(),100);`启动无限申请内存代码。

## 结果

在AndroidManifest.xml的application标签中设置`android:largeHeap="false"`得出图下结果：

![img][1]

在AndroidManifest.xml的application标签中设置`android:largeHeap="true"`得出图下结果：

![img][2]

两个结果都是app直接爆了OOM异常结束时的截图。

## 结论

每个android版本的内存限制都不一样，国内大部分rom都是定制，因此大小差异更不一样。


## 思考

当我用`adb shell dumpsys meminfo` 指令查看内存时，结果如下：

	Total RAM: 2015048 kB
	 Free RAM: 705546 kB (130082 cached pss + 408760 cached + 166704 free)
	 Used RAM: 1070272 kB (971180 used pss + 34720 buffers + 660 shmem + 63712 slab)
	 Lost RAM: 239230 kB
	     ZRAM: 280 kB physical used for 349720 kB in swap (786416 kB total swap)
	   Tuning: 192 (large 512), oom 122880 kB, restore limit 40960 kB (high-end-gfx)

其实手机内存还有富余，那为什么会报OOM呢？其实就是虚拟机对每个应用申请的内存作了限制了。

用`adb shell getprop | grep dalvik.vm.heapgrowthlimit` 命令就可以查看dalvik的vm heapsize阈值，如下：
    
    C:\Users\joy>adb shell getprop | grep dalvik.vm.heapgrowthlimit
    [dalvik.vm.heapgrowthlimit]: [192m]

    试了小米3，红米note3，三星，华为，还有一些杂牌，发现都是192M或者128M

这就是为什么没有设置大内存到了190多M就OOM的原因了，但也说明程序发生OMM并不表示RAM不足，而是因为程序申请的java heap对象超过了dalvik vm heapgrowthlimit。也就是说，在RAM充足的情况下，也可能发生OOM。这样就可以防止一个app占用所有的内存而影响其它应用内存申请，内存大家都有份^_^.那如果真的想申请大内存怎么办呢？其实可以通过jni使用C/C++来申请，这样申请的内存直接就是native的了，绕过虚拟机，当然就不受虚拟机的限制了。
---
layout: part
title: S32DS开发细节记录
original: true
typora-root-url: ..\
---

# S32DS for arm老版本的license

- 4B56-E864-CED0-E8D6

新版本的可以登录NXP官网自己注册获取

# S32DS 安装开发工具和sdk

新安装好IDE，都要安装插件，点击help里面的Install new software，把相关开发工具以及sdk都安装好

# S32DS添加自定义库文件
将xxx.c文件编译成.a文件，也就是lib库文件。
再把xxx.h文件保留，删除xxx.c文件。
按如下图片所示导入lib文件。

![](/images/s32ds/s32ds_lib_set.jpg)

# S32DS for arm老版本迁移到S32DS for S32记录
老版本的必须先clean之后，确保能编译过，然后使用export到处成zip压缩包，如下图：

![](/images/s32ds/export1.jpg)

![](/images/s32ds/export2.jpg)

然后打开新的S32DS for S32 IDE，使用import导入zip包就行，如下图：

![](/images/s32ds/import1.jpg)

![](/images/s32ds/import2.jpg)
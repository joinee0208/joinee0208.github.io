---
layout: part
title: SecureCRT日志配置
original: true
typora-root-url: ..\
---



![](/images/securecrt/log.JPG)

日志文件名

```
D:\work\log\session-%S-%Y%M%D-%h.log
```

连接时

```
[%Y%M%D_%h:%m:%s]log starting...
```

断开时

```
[%Y%M%D_%h:%m:%s]log stop!
```

在每行

```
[%h:%m:%s %t]
```
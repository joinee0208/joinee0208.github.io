---
layout: part
title: 多个pcm文件合并成烧录文件
original: true
typora-root-url: ..\
---

多个pcm文件合并成烧录文件：



```python
import os

basePath = "C:/Users/36459/Desktop/48kHz_16bit/"
pcmList = []
for file in os.listdir(basePath):
    if file.endswith(".pcm"):
        pcmList.append(file)

pcmList.sort(key=lambda name:int(name.split("_")[0]))

offset = 0
with open(file=basePath+"total.txt",mode='w') as totalTxt:
    with open(file=basePath+"total.bin",mode='wb') as totalPcm:
        for pcmFile in pcmList:
            with open(file = basePath+pcmFile,mode = 'rb') as pcm:
                length = pcm.seek(0,os.SEEK_END)
                pcm.seek(0,os.SEEK_SET)
                totalPcm.write(pcm.read(length))
                print(f"file:{pcmFile} start:{offset} end:{length}",file = totalTxt)
                offset+=length
```


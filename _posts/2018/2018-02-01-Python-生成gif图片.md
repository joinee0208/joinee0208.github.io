---
layout: post
title: Python-生成gif图片
categories: Python
description: Python-生成gif图片
keywords: Python, gif
original: true
typora-root-url: ..\..\
---

# 介绍

先了解下matplotlib做动画的方式。

参看animation模块结构图

![](/images/python/animation.JPG)

使用matplotlib做动画有两种：

- 使用FuncAnimation：Makes an animation by repeatedly calling a function func.

- 使用ArtistAnimation：Animation using a fixed set of artist objects.

之前因为做过android动画，对动画比较了解，android里动画分三种：

- 帧动画：frame animation
- 补间动画：tween animation
- 属性动画：property animation

联想以下，FuncAnimation就相当于tween animation，可以不用管动画的中间状态，仅仅使用函数来描述动画的趋势即可，ArtistAnimation就相当于frame animation，动画的每一帧都要自己来管理，类似循环播放。



# 导出gif

按动画种类，分两种途径：

- 使用FuncAnimation：配合ImageMagickWriter来导出gif，前提是要安装ImageMagick工具。
- 使用ArtistAnimation：配合pillow来导出gif，使用pip就能安装，比较轻量化。

另外，考虑到有时，我们的figure里可能有多个小图，有时要一起联动，为了控制动画的一致性，选用ArtistAnimation比较合适。

# 示例

```python
import matplotlib.pyplot as plt
import matplotlib.animation as animation
import numpy as np

def f(x, y):
    return np.sin(x) + np.cos(y)

if __name__ == "__main__":


    fig, ax = plt.subplots()
    plt.xlabel('I am x')
    plt.ylabel('I am y')

    x = np.linspace(0, 2 * np.pi, 120)
    y = np.linspace(0, 2 * np.pi, 100).reshape(-1, 1)

    # ims is a list of lists, each row is a list of artists to draw in the
    # current frame; here we are just animating one artist, the image, in
    # each frame
    ims = []
    for i in range(10):
        x += np.pi / 15.
        y += np.pi / 20.
        im = ax.imshow(f(x, y), animated=True)
        if i == 0:
            ax.imshow(f(x, y))  # show an initial one first
        ims.append([im])

    ani = animation.ArtistAnimation(fig, ims, interval=10,blit=True, repeat_delay=None,repeat = False)

    ani.save("test.gif",writer='pillow')

    plt.show()
```

运行后在本地生成gif图片，效果如下：

![](/images/python/gif.gif)
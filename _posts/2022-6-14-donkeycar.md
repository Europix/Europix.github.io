---
layout: post
title: donkeycar
categories: Blog
description: 弱智小驴车
keywords: blog
---
## 驴车小车车

说是实习 其实感觉更多是让你培养找到问题解决问题的小组合作能力

一开始做这个东西 感觉更像是 Triple E 应该干的活：

> 激光打印 + 3D打印 (后面并没有用到) + PCB打印 + 建模

一周多一点时间做车车 我大多数时间都死在弱智 python 的环境，甚至还把树莓派的 sd 卡烧了一张

![小驴车](/images/blog/donkey.jpg)

##### (Photo on June 10)

最后在建议下使用了 

> Python 3.7.2 + Tensorflow 2.2.0

给小破车识别图像 深度学习 大概是唯一用得到 CS 知识的实习了吧(叹气

在这里也感谢小组的配合 还有提供远程支援的[KevinZ小车车](https://github.com/SaigyoujiShizuka/MyDonkeyCar)

本来想在 blog 记录问题都是怎么解决的，不过做完了回看，问题太多了，我也记不清;w;

说几个犯病犯得多的吧 希望以后不要踩这个坑：

- Python手动安装记得要记得 --with-ssl && 不要用root:
''' ./configure --enable-optinizations --prefix=/usr/local/python3.7/ --with-ensurepip --with-ssl '''

- 记得多切换用户 能用sudo的命令就不要切root

- 在装py之前 一定看一下和需要包的版本能不能对应 而且如果在两个终端 需要版本一致

- 不要过度消耗自己 你还有队友 你不是懒子 不要压抑自己的情绪;w;

- 有读卡器之后不要热插拔 SD 卡，会变得不幸(我是在苏菲内存卡那里直接按一下弹出，也算是热插拔)

好了 就这么多 经历固然还是蛮重要的 踩的坑才是真正学到的 顺便给自己生日快乐
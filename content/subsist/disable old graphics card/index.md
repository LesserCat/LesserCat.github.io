---
title: "Linux 屏蔽 nouveau 驱动"
date: 2024-10-12T15:03:27+08:00
#lastmod: ""
author: "Suika"
draft: true
description: ""
tags: [  ]
categories: [  ]
# series: [ "" ]
# series_order: 
---

{{< lead >}}

{{< /lead >}}
最近要做实验，苦于没有设备，翻出来个古董笔记本想顶一下，结果装了ARCH居然会卡死。

就是进入系统就莫名其妙的卡住，啥也动不了的那种。

排查了一圈发现是显卡的问题，这机器上有一张古老的显卡，而新的开源 nouveau 驱动带不动旧的硬件，女大也不提供老设备的闭源驱动。

于是，屏蔽掉它似乎成了最可行的方案，然而试了卸载 nouveau 驱动、写 dev rules 屏蔽硬件，都没啥效果，最后在 Debian 的论坛上找到个可用的方法：

1. 打断grub的倒计时,并按 "e" 进行编辑；
2. 定位到包含 kernel 项的那一行；
3. 在结尾添加 ```nouveau.modeset=0``` 即可；
4. 之后按 "Ctrl+X " 启动系统，可以临时正常使用；
5. 进入系统修改 `/etc/default/grub` 文件，同样是添加 ```nouveau.modeset=0``` 让它永久生效；
6. 用命令 ```update-grub``` 更新 GRUB，即可永久生效

当然，也可以临时进入后安装闭源驱动以解决问题。

至此，就屏蔽掉了 nouveau 驱动，不会再卡死了。

## Reference
[Disable nouveau on Wheezy Installation?](https://forums.debian.net/viewtopic.php?t=79797)
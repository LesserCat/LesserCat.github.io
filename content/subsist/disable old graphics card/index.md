---
title: "Linux 屏蔽 nouveau 驱动"
date: 2024-10-12T15:03:27+08:00
# lastmod: ""
author: "Suika"
draft: false
description: ""
tags: [ ]
categories: [ ]
# series: [ "" ]
# series_order:
---

{{< lead >}}

{{< /lead >}}
最近要做实验，苦于没有设备，翻出来个古董笔记本想顶一下，结果死活装不上 ARCH，到了命令行就报错。

排查了一圈发现是显卡的问题，这机器上有一张古老的显卡，而新的开源 nouveau 驱动似乎是带不动旧的硬件。想要换女大的闭源驱动，也得先能进系统才行。

于是，在启动阶段就屏蔽掉它似乎成了最可行的方案，以前都是进了系统写 dev rules 或是 blacklist 来屏蔽，在安装阶段就要屏蔽是个新情况。

好在太阳底下没新鲜事，何况是这么老的显卡问题，在 Debian 的论坛上找到个可用的方法：

1.  打断 grub 的倒计时，并按 "e" 进行编辑，定位到包含 kernel 项的那一行；
2.  在结尾添加 ```nouveau.modeset=0``` 即可；
3.  之后按 "Ctrl+X " 启动系统，可以临时正常使用；

之后就可以正常安装系统了，系统安装可以选择闭源驱动以解决问题，但我用不到显卡，于是上常规手段屏蔽驱动
``` bash
vim /etc/modprobe.d/blacklist.conf
blacklist nouveau
options nouveau modeset = 0
```

如果是装好了系统再修改，而不是在安装系统阶段先修改了再装引导，需要依据系统情况更新 initramfs
比如：
```
mkinitcpio -p linux
```

当然，也可以按论坛里的方法
1.  进入系统修改 `/etc/default/grub` 文件，同样是添加 ```nouveau.modeset=0``` 让它永久生效；
2.  用命令 ```update-grub``` 更新 GRUB，即可永久生效

至此，就屏蔽掉了 nouveau 驱动，不会再卡死了，也可以安装闭源驱动了。

## Reference
 [Disable nouveau on Wheezy Installation?](https://forums.debian.net/viewtopic.php?t=79797)

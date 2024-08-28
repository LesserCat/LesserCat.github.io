---
title: "共享wifi热点"
date: 2024-08-28T10:28:28+08:00
lastmod: “”
author: "Suika"
draft: false
description: ""
tags: [ Wifi, hotspot ]
categories: [  ]
# series: [ "" ]
# series_order: 
---

{{< lead >}}
总有需求在这里很容易，在别的地方就很难
{{< /lead >}}

连着无线的笔记本，共享一个无线网络出来，对 Windows 来说很好实现，有手就行。  

然而，Linux 下就不那么容易实现了。systemd-network 固然不行，NetworkManager 也只能共享有线网络，iw 可以，但很繁琐。  

## 实现
找来找去，发现了这个项目[linux-wifi-hotspot ](https://github.com/lakinduakash/linux-wifi-hotspot)

{{< github repo="lakinduakash/linux-wifi-hotspot" >}}

最终实现的功能跟 windows 一样，也不影响其它网络管理工具，并且四舍五入也基本上有手就行。  

就是装一下，然后启动的事情，按照说明去做很方便：
```
paru -S linux-wifi-hotspot
wihotspot-gui 
```
剩下就是全图形的了

## 防火墙

项目的说明文档上没说的是防火墙，所以配成功了也可能连不上，还要加两条类似的规则：
```
firewall-cmd --zone=nm-shared --add-interface=ap0
firewall-cmd --zone=nm-shared --add-interface=ap0 --permanent
```
zone 什么的按自己的情况调整

{{< alert icon="fire" >}}
果然，没有什么是linux做不到的！
{{< /alert >}}
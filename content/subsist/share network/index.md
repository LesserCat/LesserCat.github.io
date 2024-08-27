---
title: "共享wifi热点"
date: 2024-08-27T13:58:28+08:00
lastmod: “”
author: "Suika"
draft: true
description: ""
tags: [ Wifi ]
categories: [  ]
# series: [ "" ]
# series_order: 
---

{{< lead >}}
总有需求在这里很容易，在别的地方就很难
{{< /lead >}}

连着无线的笔记本，共享一个无线网络出来，对 Windows 来说很好实现，有手就行。  

然而，Linux 下就不那么容易实现了。systemd-network 固然不行，NetworkManager 也只能共享有线网络，iw可以，但很繁琐。  

## 实现
找来找去，发现了这个项目[linux-wifi-hotspot ](https://github.com/lakinduakash/linux-wifi-hotspot)

实现功能跟windows一样，还不影响其它网络管理工具，并且四舍五入也基本上有手就行。  

就是装一下，然后启动的事情，按照说明去做很方便：
```
paru -S linux-wifi-hotspot
wihotspot-gui 
```

我主要用NetworkManager管理网络，额外配一下dnsmasq
```
/etc/NetworkManager/conf.d/dns.conf

[main]
dns=dnsmasq
```
然后`nmcli general reload`就好，dnsmasq.service本身不需要启动

## 防火墙

项目的说明文档上没说的是防火墙，所以配成功了也可能连不上，还要加两条规则：
```
firewall-cmd --zone=nm-shared --add-interface=ap0
firewall-cmd --zone=nm-shared --add-interface=ap0 --permanent
```
zone什么的按自己的情况调整。
---
title: "办公用的 Arch Linux"
date: 2024-07-06T15:43:41+08:00
lastmod: "2025-07-15T10:26:30+08:00"
author: "Suika"
draft: false
description: ""
tags: ["linux", "日常"]
categories: []
---

又到了公司换笔记本的时候。拿到新本子，第一件事当然是重装系统了，不然加了域的 Windows 11 怎么用？何况还有一堆公司的莫名其妙的软件。因此为了正常工作，系统换 Arch Linux 势在必行。

我无比赞同工作和生活的环境物理隔离，千万不要混用，而换装 ARCH 也只是为了更好的工作，我实在不会用 Windows。

至于为什么是 Arch？因为它包足够新，足够多，定制化和稳定性对个人而言也已足够。

## 安装 Arch Linux
这么多年的发展，如果不是有洁癖，安装 Arch 比原来简单多了

找个镜像下载，然后 dd 启动盘
```
wget http://mirrors.163.com/archlinux/iso/latest/archlinux-x86_64.iso
sudo dd if=/path/to/your.iso of=/dev/sdb bs=4M status=progress
sudo sync
```

官方说是 BOIS 要关闭 Secure Boot，但实际我没关闭也安装成功了，我是写这篇文章的时候才注意到官网的这个细节，如果启动不成功可以试试关闭。

然后也没必要按原来一步步参照官方 [安装指导](https://wiki.archlinux.org/title/Installation_guide)

只要启动盘没有问题，能顺利进入命令行，那么用脚本安装即可
```
archinstall
```

这种方式不需要了解太多细节，属于新手友好型，比原来的学习曲线陡降了几个维度

好像也说不上新手友好，毕竟少了很多学习的机会，不熟的话可能还会搞出错来，反倒是真的对犯懒的老人比较友好

具体使用可以参考： [https://archinstall.archlinux.page](https://archinstall.archlinux.page)
实际用起来就是个文本的安装界面，要对相关知识比较了解，就是下一步下一步的事，大大减少手动操作步骤

## 安装软件
官方有一个很长的软件列表可供参考，实际为了办公装了一些常用软件：

### 常用软件：
只记录一些需要额外配置的软件，真正常用的如 firefox、vim 和防火墙啥的都在装系统的时候顺手装过了。

中文输入法和字体：
```
sudo pacman -S fcitx5 fcitx5-chinese-addons fcitx5-gtk fcitx5-material-color
sudo pacman -S noto-fonts noto-fonts-emoji noto-fonts-cjk
fcitx5-configtool
```

装了 xfce4, 需要一个 guake 的下拉终端
```
sudo pacman -S guake
```

还有必不可少的 clash 翻墙
```
paru -S mihomo
```

vscode 也是要的
```
paru -S visual-studio-code-bin
```

自带的截图功能实在太少了，装了火焰截图

office 用了 libreoffice，配合网页的 O365 四舍五入足够了

笔记用的 obsidian，日常 PPT 都靠它

远程装了 remmina 为了远程 Windows 以及 SVN

邮件和日历，由于公司用 exchange 所以几乎别无选择得装 evolution。虽然 thunder bird 也可以但要付费插件，还是算了，打工还自己付费就很不情愿

于是：
```
sudo pacman -S ibreoffice-fresh-zh-cn remmina evolution freerdp flameshot obsidian
```

### 恶心软件
大陆自有国情在此，而且还是工作的笔记本，即时通信软件还是得装：

腾讯会议在 aur 里有现成的包，而且如果不用 wayland 的话功能都很齐全，直接装就好

国产的破软件 wayland 下工作不正常，这直接影响我用 KDE，不得不用古老的 xfce4 的原因
```
paru -S wemeet-bin
```

但企业微信就恶心了，虽然 aur 里也有个包 com.qq.weixin.work.deepin-x11，但经常崩溃，为此我自己 wine 了一个，比包里的稳定，再用 systemd-run 限制一下就安心了
```
paru -S wine wine-mono
wget https://work.weixin.qq.com/wework_admin/commdownload?platform=win&from=wwindex
wine wxwork.exe
```

直接这样 wine 的话，字体会有问题，简单的办法是找个 windows 把字体文件一股脑地拷贝过来，复杂一点的
把 windows 下的这些字体 `msyhbd.ttc/msyhl.ttc/msyh.ttc/simsun.ttc/sserife.fon` 拷贝到 `~/.wine/drive_c/windows/Fonts` 即可。此外，字体问题也可以替换 user.reg 的默认字体来解决

然后给它限制一下资源使用：
```
/usr/bin/systemd-run --user --scope --property=CPUQuota=200% --property=MemoryHigh=4G --property=MemoryMax=5G --working-directory="/home/suika/.wine/dosdevices/c:/Program Files ( x86 ) /WXWork" env WINEPREFIX="/home/suika/.wine" wine WXWork.exe
```

工作还会用 onedrive，突然发现这玩意在 linux 配好了比在 Windows 上还好用
```
paru -S onedrive-abraunegg
```

具体配置参考其 [GIT](https://github.com/abraunegg/onedrive) ，可以多个账户，配了 systemd 以后是真好用

## zsh 配置
日常还是 zsh 可玩性高一点，所以换掉 bash
```
sudo pacman -S zsh
chsh -l # list all installed shells
chsh -s /usr/bin/zsh # set zsh as default
paru -S oh-my-zsh-git zsh-syntax-highlighting zsh-autosuggestions autojump
sudo ln -s /usr/share/zsh/plugins/zsh-syntax-highlighting /usr/share/oh-my-zsh/custom/plugins/\nsudo ln -s /usr/share/zsh/plugins/zsh-autosuggestions /usr/share/oh-my-zsh/custom/plugins/
paru -S zsh-theme-powerlevel10k-git
echo 'source /usr/share/zsh-theme-powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc
p10k configure
```

## 其它

### QEMU
有时候 Windows 是绕不开的，那装个 KVM 的虚机吧。Vmware 即使免费了还是不想用，歧视 linux 用户，unity 模式仅在 linux 下不可用，那跟用 KVM 有什么区别？

KVM 的虚机装了，那 [winapps](https://github.com/Fmstrat/winapps) 也不能少，这样四舍五入就 unity 了。~~全当自我安慰吧，不然还能咋~~

虚机就是应个急，破办公机开不起，就不详细记录过程了。

然而，如果装 `qemu-full` 就多了太多没用的东西，简单一点的：
```
paru -S qemu-base qemu-common qemu-ui-spice-core qemu-hw-display-virtio-gpu qemu-hw-usb-redirect

```
这样 virtio、spice 显示什么的就都可以用了，还没那么多废物包

### 蓝牙
蓝牙的鼠标、键盘要用，得把蓝牙管理起来：
```
paru -S blueman
```
然后就可以是图形的事情了

### 杂项
另外，壁纸美化什么的看心情了，资源都很丰富，有些是习惯性的忘记了改了啥……

Anyway,就先这样吧

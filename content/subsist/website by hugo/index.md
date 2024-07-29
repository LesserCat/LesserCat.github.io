---
title: "使用Hugo和Blowfish"
date: 2024-07-08T09:32:35+08:00
author: "Suika"
draft: false
description: ""
tags: ["hugo", "折腾"]
categories: []
---
{{< lead >}}
记录一下如何搭建本站的
{{< /lead >}}
  
## 安装hugo,并下载Blowfish主题  
安装是蛮简单的：  
```
sudo pacman -S hugo
sudo pacman -S go
sudo pacman -S git
```

新建站点：
```
hugo new sit Bearcat
cd Bearcat
```

下载主题：
```
git clone https://github.com/nunocoracao/blowfish.git themes/blowfish
```

## 配置Blowfish  
首先是文档，是我在一众hugo主题里见过最齐全的文档了：[https://blowfish.page](https://blowfish.page)  

这文档详细到我不知道要写啥，毕竟我没有自定义任何东西

## 一些技巧
### tip1：  
配置主题的时候没必要看着文档一点点来，把对应主题下的exampleSite下的配置文件拿出来改改就好。  
改的时候可以开着本地环境，打开无痕浏览，一边改一边看效果  
```
hugo server --disableFastRender
```

### tip2：  
hugo写文章的时候不必每次都自己手攒头，可以定义一个模板，之后用命令创建  
需要在`archetypes`文件夹下放`default.md`，文件内容可以自定义，我的是：  
```
---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
author: "Suika"
draft: true
description: ""
tags: []
categories: []
---
```
这样在hugo网站的根目录下即可用命令创建文章了：  
```
hugo new <subdirectory of content>/index.md 
```
  
当然，如果需要不同类型的模板，可以放置不同名称的md文件，创建的时候加-k参数  
比如在`archetypes`文件有自定义的`post.md`创建文章的时候可以用：  
```
hugo new -k post <subdirectory of content>/index.md 
```
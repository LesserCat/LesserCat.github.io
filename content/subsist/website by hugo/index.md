---
title: "使用Hugo和Blowfish"
date: 2024-07-08T10:32:35+08:00
lastmod: 2024-10-16T11:46:35+08:00
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
之所以选了主题用 Blowfish 是因为文档，它有我在一众 hugo 主题里见过最齐全的文档了：[https://blowfish.page](https://blowfish.page)  

这文档详细到我不知道要写啥，一切照着做就好  

评论自己新建一个 comments.html 在 layouts/partials 下即可，hugo支持的都可以  

而我用了[cusdis](https://cusdis.com/)虽然看官网好像随时要倒闭的样子，但比其它几个评论系统胜在不需要登陆，在 Vercel 上一键搭一个也不费事，文档也够用。    

## 一些技巧
### tip1：  
配置主题的时候没必要看着文档一点点来，把对应主题下的 exampleSite 下的配置文件拿出来改改就好。  
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
  
当然，如果需要不同类型的模板，可以放置不同名称的 md 文件，创建的时候加 -k 参数  
比如在`archetypes`文件有自定义的`post.md`创建文章的时候可以用：  
```
hugo new -k post <subdirectory of content>/index.md 
```

## Addition
有人问评论区和碎碎念的配置，我本来觉得不用写的，但有人有兴趣就大致写一下
### 评论区
[cusdis](https://cusdis.com/)其实挺容易的，如果不自己搭就更容易了，注册一个账号，它就显示个 dashboard，而在 dashboard 的上方有个 Embeded code 按钮，它直接给出一段可用的 js 脚本，包含了官方服务的地址和用户的 ID

但由于框架是 Hugo 所以不能直接用，要按 Hugo 的文档改一下：
```
<div id="cusdis_thread"
  data-host="https://cusdis.com"
  data-app-id=""
  data-page-id="{{ .File.UniqueID }}"
  data-page-url="{{ .RelPermalink }}"
  data-page-title="{{ .Title }}"
></div>
```
这样才能获取到正确的文章链接，保证评论出现在正确的地方  

**data-app-id** 一定用自己的，否则就是抄作业把名字也抄走了

而默认给的脚本是英文的，可以加一条换成中文，当然也可以不加
```
{{- $js := resources.GetRemote "https://cusdis.com/js/widget/lang/zh-cn.js" | js.Build (dict "minify" true) -}}
```

再具体的请参考本站源码 `layouts\partials\comments.html`

### 碎碎念
碎碎念的实现借助了两个服务：Github Action 和 Telegram bot   
申请 Telegram Bot就不说了，Github需要在仓库的设置里添加两个变量：  
Repo -> Settings -> Secrets and variables -> Action -> New Repository secret  
分别加 BOT_TOKEN 和 CHANNEL_ID 按变量名可知变量内容  

然后就是复制了，碎碎念的 shortcode 基于 Blowfish 主题的timeline，只改了一点点 CSS  
名为post 的 shortcode 位于 layouts/shortcodes 下，搬用即可，然后跟调用其它 shortcode 一样，在文章中嵌入这个 shortcode 就好。  
比如本站做的 content/murmur/index.md 一样，它展示了一个 json 文件的，文件位于 /data/tg 下，是由前面 github action 生成的，生成的 python 代码是根下的 update_posts.py。  

感觉像是在写流水账，Anyway，就这样吧
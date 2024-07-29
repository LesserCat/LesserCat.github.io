---
title: "发布环境"
date: 2024-07-29T15:24:30+08:00
author: "Suika"
draft: false
description: ""
tags: [hugo, github]
categories: []
---

{{< lead >}}

{{< /lead >}}

断断续续地在本地调试了一段时间，想着先把它上线吧

## GIT同步
到Github上创建个跟用户名同名的repo,然后把代码同步过去：
```
rm -rf themes/blowfish/ 
git init .
git submodule add -b main https://github.com/nunocoracao/blowfish.git themes/blowfish
git remote add origin git@github.com:LesserCat/LesserCat.git
git add .
git commit -m "init"
git push -u origin main
```
这之前当然是要配基础环境到git,太基础就不写了  

## Github Action
在仓库的Actions里添加workflow
Actions -> New workflow -> 搜索hugo -Configure -> Commit changes

然后等待action完成即可访问username.github.io

https://lessercat.github.io/

## CloudFlare
我更喜欢Cloudflare Page,域名可以自由一点，就更容易了  

创建一个新的PAGE与git关联，在Project Name那里添一个可用的喜欢的域名

Framework preset选择hugo，就可以部署了，速度比Github Page快多了，后面扩展性也更好。

## Reference
https://developers.cloudflare.com/pages/framework-guides/deploy-a-hugo-site/
https://docs.github.com/en/pages/quickstart
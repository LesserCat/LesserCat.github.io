---
title: "发布环境"
date: 2024-07-29T15:24:30+08:00
lastmod: "2024-07-31T09:51:30+08:00"
author: "Suika"
draft: false
description: ""
tags: [hugo, github]
categories: []
---

{{< lead >}}

{{< /lead >}}

断断续续地在本地调试了一段时间，想着先把它上线吧。

但备案是不可能备案的，那就 serverless 吧，额外加个域名的意义也不大，也不指望 SEO 排名。

## GIT 同步
到 Github 上创建个跟用户名同名的 repo,然后把代码同步过去：
```
rm -rf themes/blowfish/
git init .
git submodule add -b main https://github.com/nunocoracao/blowfish.git themes/blowfish
git remote add origin git@github.com: LesserCat/LesserCat.git
git add .
git commit -m "init"
git push -u origin main
```
这之前当然是要配基础环境到 git，太基础就不写了

## Github Action
在仓库的 Actions 里添加 workflow
Actions -> New workflow -> 搜索 hugo -Configure -> Commit changes

然后等待 action 完成即可访问 [username.github.io](https://lessercat.github.io/)

**UPDATE:**
此时虽然能访问，但在 actions 的任务里总有一个 pages build and deployment 的任务是失败，还失败的莫名其妙

需要去项目的设置里把部署方从 Depoly from a branch 式改成 Github Actions，路径：
Settings -> Pages -> Build and deployment -> Github Actions

## CloudFlare
我更喜欢 Cloudflare Page,域名可以自由一点，就更容易了

创建一个新的 PAGE 与 Git 关联，在 Project Name 那里添一个可用的喜欢的域名

Framework preset 选择 hugo，就可以部署了，速度比 Github Page 快多了，也不像 Github 一样各处都被墙。

## Reference
https://developers.cloudflare.com/pages/framework-guides/deploy-a-hugo-site/
https://docs.github.com/en/pages/quickstart

---
title: hexo发布
date: 2017-04-30 17:48:34
tags: [hexo,装逼利器]
categories: HEXO
---
## 发布hexo
发布到github上一个账号只能发布一次，创建新的github库名字必须是
`<github用户名>.github.io`
## 配置到github
在_config.yml中找到`deploy`中添加一下内容
<!--more-->
```bash
deploy:
  type: git
  repository: https://github.com/<github用户名>/<github用户名>.github.io.git
  branch: master
```
## 下载发布插件
```bash
npm install hexo-deployer-git --save
```
## 发布
```bash
hexo g #生成静态页
hexo d #发布博客
```

---
title: git发布静态网站
date: 2017-04-29 15:59:01
tags: [git,前端笔记]
categories: Git
---
## 在github上发布静态网站
- 必须在当前项目下建立一个`gh-pages`的分支
- 将我们需要发布的内容推送到`gh-pages`这个分支上
- 推送到远程仓库上即可
- github会给你一个在线地址

## 执行步骤
- git checkout -b gh-pages
- touch index.html
- git add .
- git commit -m "add index.html"
- git push origin gh-pages
-在github中的setting页可以查找到网址+文件名即可（默认会展示index.html）

## 获取自己仓库的代码
```
git clone <地址>
```

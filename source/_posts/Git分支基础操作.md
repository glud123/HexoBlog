---
title: Git分支基础操作
date: 2017-04-29 12:14:41
tags: [git,前端笔记]
categories: Git
---
## 查看当前git分支
```
git branch
```
## 创建分支
```
git branch <分支名>
```
<!--more-->
## 切换分支
```
git checkout <分支名>
```
## 删除分支
注：删除分支之前一定确定要删除的分支，不是当前所在分支，切换其他分支之后即可删除
```
git branch -D <分支名>
```
## 创建并切换指定分支
```
git checkout -b <分支名>
```
## 分支合并 用主干分支合并分支
注：默认master是主干
```
git merge <分支名>
```

> 合并分支后 被合并的分支删除即可

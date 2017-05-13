---
title: git文件对比
date: 2017-04-29 11:00:26
tags: [git,前端笔记]
categories: Git
---
## 查看版本信息
```
git log
```
或者
```
git log --oneline
```
<!--more-->
## 文件内容比对
- 工作区和暂存区文件比较
```
git diff
```
- 暂存区和版本文件比较
```
git diff --cached
```
- 工作区和版本库比较
```
git diff master
```

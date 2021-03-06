---
title: Git文件回滚
date: 2017-04-29 11:18:08
tags: [git,前端笔记]
categories: Git
---
## 跨过暂存区直接将本地文件提交到版本库

如果执行过一次增加到版本库 `git commit -a -m "更新内容描述"`

## 从暂存区拉取历史文件
```bash
# 拉取全部文件
git checkout .
# 拉取指定文件
git checkout <文件名>
```
<!--more-->

## 从版本库将文件某一版本回滚到暂存区和工作区
```bash
git reset --hard <版本号>
```

## 查看所有操作的版本号
```bash
git reflog
```

## 搜索带有<需要查询的关键字>的版本号
```bash
git log --grep=<需要查询的关键字>
```

## 搜索<作者/用户名>提交的历史版本
```bash
git log --author=<作者/用户名>
```

## 加入暂存区后，返回上一次的添加 （删除本次的 `add`）
```bash
# 返回所有文件
git reset HEAD .
# 返回指定文件
git reset HEAD <文件名>
```
---
title: git文件回滚
date: 2017-04-29 11:18:08
tags: [git,前端笔记]
categories: Git
---
## 跨过暂存区直接将本地文件提交到版本库

如果执行过一次增加到版本库 `git commit -a -m "更新内容描述"`

## 从暂存区拉取历史文件
- 全部拉取回来
```
git checkout .
```

- 拉取某一个文件
```
git checkout <文件名>
```

## 从版本库将文件某一版本回滚到暂存区和工作区
```
git reset --hard <版本号>
```

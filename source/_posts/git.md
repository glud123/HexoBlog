---
title: git
date: 2017-04-29 10:04:24
tags: [git,前端笔记]
categories: Git
---
## pwd
  print working directory 打印当前工作目录

## 告诉git当前用户

- 用git第一次需要配置 以后都不需要

```
git config --global user.name <your name>
git config --global user.email <your email>
git config list 查看配置列表

```
## 初始化文件夹（告诉git那个文件夹归git所管理）

```
git init
```
## 打开文件夹
```
cd
```

## 删除文件夹  循环删除（递归删除）
```
rm -rf <文件夹名>
```
## 删除文件
```
rm <文件名>
```

## macOS中查看隐藏文件夹
```
ls -a
```
## 新建文件
```
touch <文件名>
```
## 查看文件名
```
cat <文件名>
```
## 编辑文件 进入vi编辑模式
```
vi <文件名>
```
## 在vi模式下
- 按 `i` 键 进入编辑模式
- 编辑文件
- 按  `esc` 键，退出编辑模式
- `:wq` 保存并退出
- `:q!` 强制退出，不保存

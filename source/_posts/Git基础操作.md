---
title: Git基础操作
date: 2017-04-29 10:04:24
tags: [git,前端笔记]
categories: Git
---
## 设置当前用户信息

用git第一次需要配置 以后都不需要

```bash
git config --global user.name <your name>
git config --global user.email <your email>
git config list 查看配置列表
```
<!--more-->

## 初始化 git 本地工作仓库

```bash
git init
```
## 打开文件夹
```bash
cd
```

## 删除文件夹及其子文件  循环删除（递归删除）
```bash
rm -rf <文件夹名>
```
## 删除文件
```bash
rm <文件名>
```

## macOS中查看隐藏文件夹
```bash
ls -a
```
## 新建文件
```bash
touch <文件名>
```
## 查看文件名
```bash
cat <文件名>
```
## 编辑文件 进入vi编辑模式
```bash
vi <文件名>
```
## Vim 简单命令
- 按 `a` 键 进入编辑模式
- 按  `esc` 键，退出编辑模式
- `:wq` 保存并退出
- `:q!` 强制退出，不保存

## 工作区进行提交到暂存区
提交全部

```bash
git add .
# 或者
git add -A
```

提交单个文件

```bash
git add <文件名>
```

## 暂存区提交到版本库
```bash
git commit -m "更新内容描述"
```
## 提交到版本库
```bash
git push
```
## 查看文件所在区的提交状态
```bash
git status
```

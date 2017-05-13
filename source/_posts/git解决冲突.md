---
title: git解决冲突
date: 2017-04-29 12:46:48
tags: [git，前端笔记]
categories: Git
---
## 处理冲突
由于两个分支改变了相同的文件，但是文件内容不同，这时候需要手动吹，再次提交，就可以完成合并
> 模块化开发，可以降低冲突的发生

## 查看git提交图谱
```
git log --graph
```
<!--more-->
通过上下方向键拉倒提交日志最低端，按`Q`键退出提交日志窗口。

## github注册之后需要配置邮箱

## 创建有内容的文件
```
echo "重写的内容" > README.md
echo "需要追加的内容" >> README.md
```

## 本地仓库与远程仓库进行关联
```
git remote add <名字> <地址>
```

## 查看远程仓库的连接列表
```
git remote -v
```
## 删除远程仓库的连接
```
git remote <连接名>
```

## 推送到远程仓库
-u 为upstream  设置之后下次推送时可以使用简写
```
git push origin master -u
简写
git push
```

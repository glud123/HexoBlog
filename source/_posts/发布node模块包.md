---
title: 发布node模块包
date: 2017-04-30 15:06:53
tags: [node,node基础]
categories: node
---
## 发布自己的包

> 包中必须要有package.json文件

- 多个js文件组成的就叫包

- 通常都存在一个入口文件

- 发布这个文件夹（发布的包不能和已经存在的包相同）

  <!--more-->
## 向 npm 官网发包
如果之前使用 nrm 切换过 npm 的源地址，应先切换到官方源地址

```bash
nrm use npm
```
如果有账号表示登录，没有账号表示新建账号

```bash
npm addUser
```
发布

```bash
npm publish
```
删除模块

```bash
npm unpublish <模块名>@<版本号>
```
查询是否登录过

```bash
npm whoami
```
## 使用idoc进行文档展示

安装idoc

```bash
npm install idoc -g
```

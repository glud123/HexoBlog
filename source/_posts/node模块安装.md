---
title: node模块安装
date: 2017-04-30 11:39:38
tags: [node,node基础]
categories: node
---
## 模块
- 文件模块
- 内置模块
- 第三方模块
### 模块安装
- npm root -g 可以查看安装到哪里去了
- 全局安装 -g 在命令行下使用
- nrm 切换源的工具
> 默认下载从官方下载npm，nrm可以切换国内数据源
- 全局安装nrm
<!--more-->
```bash
npm install -g nrm
```
- 列出所有可用的源
```bash
nrm ls
```
- 测试源的速度
```bash
nrm test
```
- 添加数据源
```bash
nrm add <源名字> <地址> 
```
- 删除数据源
```bash
nrm del <源名字> 
```
- 使用数据源
```bash
nrm use <源名字> 
```
### 本地安装 （如果切换到淘宝后，以后都是通过淘宝进行安装）
- 本地安装主要在项目中使用
> 本地安装会默认安装到node_modules 中,如果没有初始化package.json可能会安装上一node_modules目录
```bash
npm init -y
npm install jquery --save #安装
npm uninstall jquery --save #卸载
```

### 项目依赖
- 上线需要开发时也需要 mime
```
--save 可以简写 -S
```
### 开发依赖
- 上线不需要，开发时需要 webpack gulp
```
--save-dev  可以简写 -D
```
### 安装依赖
- 当我们文件上传到github上，会node_module文件忽略掉，别人下载代码后需要执行npm install 将所有依赖进行安装
```bash
npm install
```
### 安装指定版本
```bash
npm install <依赖名>@版本号
```
### 查看指定版本号
```bash
npm info <依赖名>
```
## yarn
- 也是安装模块的方式，和npm一样。
```bash
npm install yarn -g
```
- 初始化package.json
```bash
yarn init -y
```
- 安装模块
如果是项目依赖则不用标注，如果是开发依赖需要标注（-dev）
```bash
yarn add <依赖名> //项目依赖
yarn add <依赖名> -dev //开发依赖
```
- 删除模块
```bash
yarn remove <依赖名> (-dev)
```
- 找回模块
```bash
yarn install
```

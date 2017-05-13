---
title: WebStorm搭建本地授权服务
date: 2017-02-09 08:55:44
tags: [WebStorm,PhpStorm,开发工具]
categories: 开发工具
---
> ### 本地搭建各种storm License Server ,建立自己的本地授权服务器。告别那些恶心的注册码。

## 适用情况：理论上支持所有Jetbrains产品，截止到目前所有版本都可正常激活。
### 1、下载下面的文件
<!--more-->
#### 链接地址：
 http://ol31uv24o.bkt.clouddn.com/IDEALicenseServer.zip
### 2、解压文件，找到自己的操作版本。

以下使用windows64位系统做示例。
找到![图1](http://ol320xopj.bkt.clouddn.com/blog-webstorm-01.jpg)

双击运行 ***IntelliJIDEALicenseServer_windows_amd64.exe***

![图2](http://ol320xopj.bkt.clouddn.com/blog-webstorm-02.jpg)

### 3、之前如果通过其他方式注册的，可以在 ***WebStorm*** 中的 ***Helep*** 选项下 ***Register...*** 选项中进行重新注册。

![图3](http://ol320xopj.bkt.clouddn.com/blog-webstorm-03.jpg)

### 4、在注册界面选择 ***License server*** 授权服务器，填写 ***http://127.0.0.1:41017***,然后点击 “Activate” ,如下图
![图4](http://ol320xopj.bkt.clouddn.com/blog-webstorm-04.jpg)

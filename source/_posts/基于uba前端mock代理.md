---
title: 基于uba前端mock代理
date: 2018-03-05 10:28:40
tags: [uba,mock]
categories: node
---
## mock 代理

>1. uba 自带 mock 代理插件，默认配置为开启代理功能。
>2. 数据模拟功能对开发环境有效，生产环境无效。

#### 1.数据模拟配置
- 数据模拟功能默认开启。
> `uba.config.js` 配置文件相关配置项
```
// 代理模式切换，enable:true启用代理，数据模拟失效.只对开发环境有效
const proxyConfig = [{
  enable : false, // false -> 关闭代理，数据模拟开始
  router: "/", // 接口地址路由
  url: "cnodejs.org",
  options : {
    filter : function(req,res){
      return (req.url.indexOf("webpack_hmr") > -1 ? false : true);
    }
  }
}];
```
<!--more-->
- 配置 mock 配置文件
> 模板根目录下`uba.mock.js`文件为 mock 配置文件。
```
/*
 * 配置数据模拟
*/

module.exports = {
  "GET": [{ // 需要模拟的 get 请求
    "/User/Get": "./mock/api/user/get.json"
  }],
  "POST": [{ // 需要模拟的 post 请求
    "/User/Post": "./mock/api/user/post.json"
  }]
}
```
#### 2.代理配置
> 1.mock 代理主要解决跨域请求的问题。
>
> 2.开启代理服务，需要前端开发环境和后台开发环境在相同网络当中（即相同局域网内同一网段）。并能正常 `ping` 通。

> `uba.config.js` 配置文件相关配置项

```
// 代理模式切换，enable:true启用代理，数据模拟失效.只对开发环境有效
const proxyConfig = [{
  enable : true, // true -> 开启代理，数据模拟失效
  router: "/", // 接口地址路由
  url: "192.168.137.1:8080", // 设置后端接口服务ip地址
  options : {
    filter : function(req,res){
      return (req.url.indexOf("webpack_hmr") > -1 ? false : true);
    }
  }
}];
```
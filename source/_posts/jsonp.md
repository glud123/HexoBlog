---
title: jsonp跨域
date: 2017-04-23 12:47:33
tags: [node,js,jsonp]
categories: jsonp
---
## 两个网站要共享数据
jsonp 为跨域的最常见的手段（不能发送数据，只支持get请求）
ajax会有限制问题，会出现不允许访问。
jsonp,cros,postMessage(iframe),websocket;
- jsonp:scritp 不会导致跨域问题，我的网站引用你的网站的js,img,css,不用img和css的原因是默认会被转成图片和样式，img常用作记录访问页面的次数，统计请求这样图片的次数。
> jsonp是通过src跨域的，并且只支持get请求。
>
> 由于src没有访问限制，可以通过src引用其他网站的数据（此网站必须通过这样的接口）
- cros 让被访问的服务器 允许跨域即可 （需要服务端支持）
- iftame 跨域 html5 提供的API
- websocket html5 提供的API
## jsonp 的原理
- 1.在当前js中声明一个全局函数
function fn(){}
- 2.动态创建script标签
通过script 标签引用其他网址并且将这个全局函数名字传递给后台

```
<script src ="http://localhost:3000/getuser?callback=fn">
```
- 3.后台返回函数执行
`
fn('{name:1}');
`

---
title: node-EventEmitter方法
date: 2017-04-24 22:48:38
tags: [node,js,nodeAPI]
categories: node
---
## EventEmitter方法
- `addListener(eventName,listener)` 对指定事件绑定监听

- `on(enentName,listener)` 对指定事件绑定监听同 `addListener(eventName,listener)`

- `once(eventName,listener)` 对指定事件仅绑定一次
<!--more-->
- `removeListener(eventName，listener)` 解除绑定监听

- `removeAllListener(eventName)` 解除所有绑定的监听事件

- `emit(eventName,args)` 触发事件第一个是事件类型 args是传入的参数

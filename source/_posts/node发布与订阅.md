---
title: node发布与订阅-观察者模式
date: 2017-04-30 16:01:00
tags: [node,node基础]
categories: node
---
## 发布订阅模式
- 一种一对多的关系，订阅事件和发布事件
- {'水开了':['泡面','洗澡']}

## 绑定事件 on
维护一个一对多的关系{"被监听的事件名":[事件1,事件2,事件3]}
<!--more-->
## 发射事件 emit
当触发事件后会将绑定的事件依次触发
通过发射的事件名，将数组里面的参数依次执行

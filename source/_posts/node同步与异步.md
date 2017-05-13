---
title: node同步与异步
date: 2017-04-30 09:46:51
tags: [node]
categories: node
---
## 同步与异步
node中异步包括：
- setTimeout
- callback
- process
- setImmediate
- setInterval
<!--more-->
## 阻塞非阻塞
针对的内核，厨师，如果厨师不释放掉服务员，主线程不可能异步
- 非阻塞是异步的前置条件

> 主线是单线程的，内核是多线程

## 单线程&多线程
- 一个个发短信
- 群发短信多线程

> 进程包括线程，在node中一个进程只能包括一个线程，node可以多开多个线程，开多个进程（子进程）

## 事件循环
- 事件驱动

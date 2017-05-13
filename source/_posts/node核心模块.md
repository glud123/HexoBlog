---
title: node核心模块
date: 2017-04-30 12:37:18
tags: [node，node基础]
categories: node
---
## 核心模块
引用核心模块不需要安装，也不需要使用./或者../来引入

### util模块
util 最主要的功能是继承，判断类型
>js中的继承call，原型继承，new，extends
- Obj.create() es5的继承 只继承公有
```
Sun.prototype = Obj.create(Parent.prototype)
```
<!--more-->
- Object.setPrototypeOf() es6只继承公有
```
Object.setPrototypeOf(Sun.prototype,Parent.prototype);
```
原理Sun.prototype.__proto__ = Parent.prototype
- call 只继承私有属性
- Sun.prototype = new Parent() 可以继承公有和私有的方法 缺点不能传递参数
- Sun.prototype.__proto__ = Parent.prototype 通过原型链连接父亲的原型

- util.inherits(Child,Parent);   //node中的继承（继承公有属性）

### events模块
常用的包括 on，emit，once，removeListener，removeAllListener

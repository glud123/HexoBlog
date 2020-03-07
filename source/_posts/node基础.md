---
title: node基础（一）
date: 2017-04-30 09:58:14
tags: [node,node基础]
categories: node
---
> node 默认执行一个‘文件’ 时，就会套一层闭包函数，所有代码都是在个函数中执行的，默认这个函数提供5个参数，这五个参数可以直接使用，不用声明可以直接使用exports,require,module,__filename,dirname

- 1.process 进程对象
- 2.Buffer 缓冲区，表示内存中的数据（16进制）
- 3.clearImmediate , setImmediate
- 4.clearInterval , setInterval
- 5.clearTimeout , setTimeout
- 6.console
<!--more-->
## console 顺序不定的
- console.log('log');
- console.error('error');
- console.warn('warn');
- console.info('info');

## setTimeout 异步
- (1)这里的this是setTimeout 自己
console.log(this);//这里的this被外层的闭包改掉了，是个{}
(function(){
  console.log(this);
})();//这里的this是global
> 改变this指向的方式
  - 1.call apply
  - 2.bind
  - 3.var that = this;
  - 4.arrow func 即箭头函数

- (2)setTimeout(function(参数1,参数2){
  console.log(参数1，参数2);
},1000,'参数1','参数2')；

## 高阶函数
> let a = b => c => b + c;
console.log(a(2)(3)); //5

## setImmediate 不能传递时间
等待同步代码执行后调用，没有写时间的setTimeout
```javascript
setImmediate(function () {
    console.log('setImmediate')
});
setTimeout(function () {
    console.log('setTimeout')
});
console.log('ok');//默认setTimeout 有可能比 setImmediate 先执行
```
## process 代码执行时会开一个进程，代码运行完成后进程就结束了
```javascript
setInterval(function () {
    //process.pid//当前进程id
     console.log(process.pid);
     //process.kill(13140); //杀掉进程
     //process.exit();//退出进程
},1000);
```
```javascript
process.nextTick(function () {
    console.log('nextTick')
});//是异步的函数
```
> nextTick > setImmediate > setTimeout

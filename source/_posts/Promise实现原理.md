---
title: Promise实现原理
date: 2018-03-16 22:17:09
tags: [前端，基础]
categories: es6
---
## Promise实现原理
> Promise 是一个类，高版本浏览器已经实现了 Promise 类

Promise 需要传递一个执行函数即 executor 

```javascript
/**
 * @param {Function} executor 即执行函数
 */
new Promise(function(){
    console.log(1);
})

// => 1
```
<!-- more -->

executor 包含两个参数，分别是 resolve (解决)、 reject (拒绝) 

如果 resolve 执行成功，不会运行 reject

```javascript
/**
 * @param {Function} resolve 成功的回调
 * @param {Function} reject 失败的回调
 */
let p = new Promise(function(resolve,reject){
    // resolve('ok');
    reject('err');
    // 当在执行函数中抛出错误的时候，会执行失败的回调
    // throw new Error();
})

// p 代表的是 Promise 实例
p.then(function(data){
    console.log(data);
},function(err){
    console.log(err);
});

// => ‘err’
```

### 以下是 Promise 基本功能实现

```javascript
/**
 * 
 * @param {Function} executor 执行函数
 */
function Promise(executor) {
	let _this = this;
	// 初始状态
	_this.status = 'pending';
	// 执行成功的初始值
	_this.value = undefined;
	// 执行失败的初始值
	_this.reason = undefined;
	// 存储多个成功回调
	_this.reslovedCallBacks = [];
	// 存储多个失败回调
	_this.rejectedCallBacks = [];
	function resolve(value) {
		if (_this.status === 'pending') {
			_this.status = 'resloved';
			_this.value === value;
			_this.reslovedCallBacks.forEach((element) => {
				element(_this.value);
			});
		}
	}
	function reject(reason) {
		if (_this.status === 'pending') {
			_this.status = 'rejected';
			_this.reason === reason;
			_this.rejectedCallBacks.forEach((element) => {
				element(_this.reason);
			});
		}
	}
	try {
		executor(resolve, reject);
	} catch (e) {
		// 执行函数报错时，直接抛出错误给 reject 函数
		reject(e);
	}
}
/**
 * 
 * @param {Function} onFullfilled 成功的回调
 * @param {Function} onRejected 失败的回调
 */
Promise.prototype.then = function(onFullfilled, onRejected) {
	// 当执行函数为等待状态的时候 将 onFullfilled、onRejected 进行存储
	if (this.status === 'pending') {
		this.resloveCallBacks.push(onFullfilled);
		this.rejectCallBacks.push(onRejected);
	}
	// 当执行函数状态为 resloved 时 执行成功回调函数
	if (this.status === 'resloved') {
		onFullfilled(this.value);
	}
	// 当执行函数状态为 rejected 时 执行失败回调函数
	if (this.status === 'rejected') {
		onRejected(this.reason);
	}
};
```

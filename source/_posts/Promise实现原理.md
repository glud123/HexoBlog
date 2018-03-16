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
### 以下是 Promise 基本功能实现

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
 * @param {Function} onFulfilled 成功的回调
 * @param {Function} onRejected 失败的回调
 */
Promise.prototype.then = function(onFulfilled, onRejected) {
    let _this = this;
	// 当执行函数为等待状态的时候 将 onFulfilled、onRejected 进行存储
	if (_this.status === 'pending') {
		_this.resloveCallBacks.push(onFulfilled);
		_this.rejectCallBacks.push(onRejected);
	}
	// 当执行函数状态为 resloved 时 执行成功回调函数
	if (_this.status === 'resloved') {
		onFullfilled(_this.value);
	}
	// 当执行函数状态为 rejected 时 执行失败回调函数
	if (_this.status === 'rejected') {
		onRejected(_this.reason);
	}
};
```
## Promise 的链式调用

> 链式调用的特点，会将第一个 then 中不论成功或者失败的返回值，作为下一个 then 中成功的回调函数的参数

链式调用：依靠的是返回一个新的 Promise 实例

### 链式调用的基本功能实现

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
 * @param {Function} onFulfilled 成功的回调
 * @param {Function} onRejected 失败的回调
 */
Promise.prototype.then = function(onFulfilled, onRejected) {
	let _this = this;
	let promise2;
	// 当执行函数为等待状态的时候 将 onFulfilled、onRejected 进行存储
	if (_this.status === 'pending') {
		// 之所以在返回一个新的 Promise 是因为还要继续走下一个 then 中成功和失败的回调
		return (promise2 = new Promise(function(resolve, reject) {
			_this.resloveCallBacks.push(function() {
				let x = onFulfilled(_this.value);
				if (x instanceof Promise) {
					// 执行新的 Promise
					x.then(resolve, reject);
				} else {
					resolve(x);
				}
			});
			_this.rejectCallBacks.push(function() {
				let x = onRejected(_this.reason);
				if (x instanceof Promise) {
					x.then(resolve, reject);
				} else {
					resolve(x);
				}
			});
		}));
	}
	// 当执行函数状态为 resloved 时 执行成功回调函数
	if (_this.status === 'resloved') {
		return (promise2 = new Promise(function(resolve, reject) {
			let x = onFulfilled(_this.value);
			// 判断成功的回调执行结果是否为 Promise 函数，如果是执行新的 Promise 实例 then 方法，如果不是直接执行 Promise 中成功的方法
			if (x instanceof Promise) {
				x.then(resolve, reject);
			} else {
				resolve(x);
			}
		}));
	}
	// 当执行函数状态为 rejected 时 执行失败回调函数
	if (_this.status === 'rejected') {
		return (promise2 = new Promise(function(resolve, reject) {
			let x = onRejected(_this.reason);
			if (x instanceof Promise) {
				x.then(resolve, reject);
			} else {
				resolve(x);
			}
		}));
	}
};
```


---
title: Promise实现原理
date: 2018-03-16 22:17:09
tags: [前端，基础]
categories: es6
---
# Promise实现原理

> [MDN 对 Promise 的描述](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)

## Promise 语法

```javascript
new Promise( function(resolve, reject) {...} /* executor */  );
```

## 参数

#### **executor**

executor是带有 `resolve` 和 `reject` 两个参数的函数 。Promise 构造函数执行时立即调用`executor` 函数， `resolve` 和 `reject` 两个函数作为参数传递给`executor`（ executor 函数在 Promise 构造函数返回所建 promise 实例对象前被调用）。`resolve` 和 `reject` 函数被调用时，分别将 promise 的状态改为 *fulfilled（*完成）或 rejected（失败）。executor 内部通常会执行一些异步操作，一旦异步操作执行完毕(可能成功/失败)，要么调用 resolve 函数来将 promise 状态改成 *fulfilled*，要么调用`reject` 函数将promise 的状态改为 rejected 。如果在executor函数中抛出一个错误，那么该 promise 状态为rejected 。executor 函数的返回值被忽略。

## 使用 Promise

```javascript

new Promise(function(){
	console.log(1);
});
// => 1

new Promise(function(resolve, reject){
	resolve(1);
}).then((value)=>{
	console.log(value);
});
// => 1

new Promise(function(resolve, reject){
	reject(1);
}).then((fulfilled)=>{
	console.log("fulfilled:", fulfilled);
}, (rejected)=>{
	console.log("rejected:", rejected); 
});
// => rejected: 1

new Promise(function(resolve, reject){
	reject(1);
}).then((fulfilled)=>{
  console.log("fulfilled:", fulfilled);
}, (rejected)=>{
  console.log("rejected:", rejected); 
  return 2
}).then((fulfilled)=>{
  console.log("fulfilled:", fulfilled);
}).catch((err)=>{
  console.log("catch:", err);
});
// => rejected: 1
// => fulfilled: 2

new Promise(function(resolve, reject){
	reject(1);
}).then((fulfilled)=>{
  console.log("fulfilled:", fulfilled);
}).then((fulfilled)=>{
  console.log("fulfilled:", fulfilled);
}).catch((err)=>{
  console.log("catch:", err);
});
// => catch: 1

```

<!-- more -->

## 分析

通过上面对 Promise 的简单使用，我们可以总结出以下几点：

- Promise 接收一个函数（executor）
- executor 包含两个参数，分别是 resolve (解决)、reject（拒绝）
- Promise 有三个状态 pending（初始状态，既不是成功，也不是失败状态）、fulfilled（意味着操作成功完成）、rejected（意味着操作失败）
- 可以调用 `then` 属性，并接收两个函数参数，这两个函数分别接收 Promise 的 resolve 、reject 传递的参数

## Promise 基本功能实现

> 链式调用的特点，会将第一个 then 中不论成功或者失败的返回值，作为下一个 then 中成功的回调函数的参数

链式调用：依靠的是返回一个新的 Promise 实例

```javascript
/**
 * @param {Function} executor 执行函数
 */
const PENDING = "pending";
const FULFILLED = "fulfilled";
cosnt REJECTED = "rejected";
function promise(executor) {
	// 初始状态
	this.status = PENDING;
	// 初始值
	this.result = null;
	function resolve(value) {
		if (this.status === PENDING) {
			this.status = FULFILLED;
			this.result = value;
		}
	}
	function reject(reason) {
		if (this.status === PENDING) {
			this.status = REJECTED;
			this.result = reason;
		}
	}
	try {
		executor(resolve.bind(this), reject.bind(this));
	} catch (e) {
		// 执行函数报错时，直接抛出错误给 reject 函数
		reject(e);
	}
}
/**
 * promise then 属性
 * @param {Function} onFulfilled 成功的回调
 * @param {Function} onRejected 失败的回调
 */
Object.defineProperty(promise.prototype, "then", {
  value: function (onFulfilled, onRejected){
    // 当执行函数状态为 resloved 时 执行成功回调函数
    if(this.status === FULFILLED){
       this.result = onFulfilled(this.result);
       return new promise((resolve, reject)=>{
         resolve(this.result);
       };
    }
    // 当执行函数状态为 rejected 时 执行失败回调函数
    if(this.status === REJECTED){
      if(onRejected && typeof onRejected === "function"){
        this.result = onRejected(this.result);
        return new promise((resolve, reject)=>{
          resolve(this.result);
        });
      }else{
        return new promise((resolve, reject)=>{
          reject(this.result);
        });
      }
    }
  }
});
/**
 * promise catch 属性
 * @param {Function} onRejected 失败的回调
 */
Object.defineProperty(promise.prototype, "catch", {
  value: function (onRejected){
    if(this.status === REJECTED){
       onRejected(this.result);
    }
  }
});
```
以上只是实现 Promise 在同步状态下的链式调用，当 executor 内部存在异步操作时或者 不能及时调用 Promise 中 resolve、reject 方法时，promise 函数并不能按预期执行。

## Promise 完整实现

### 

```javascript
/**
 * @param {Function} executor 执行函数
 */
const PENDING = "pending";
const FULFILLED = "fulfilled";
const REJECTED = "rejected";
function promise(executor) {
  // 初始状态
  this.status = PENDING;
  // 初始值
  this.result = null;
  // 异步场景下存储多个成功回调
  this._fulfilledCallbacks = [];
  // 异步场景下存储多个失败回调
  this._rejectedCallbacks = [];

  function resolve(value) {
    if (this.status === PENDING || this.status === REJECTED) {
      this.status = FULFILLED;
      this.result = value;
      let _fulfilledCallback;
      while ((_fulfilledCallback = this._fulfilledCallbacks.shift())) {
        this.result = _fulfilledCallback(this.result);
      }
    }
  }
  function reject(value) {
    if (this.status === PENDING) {
      this.status = REJECTED;
      this.result = value;
      for (let index = 0; index < this._rejectedCallbacks.length; index++) {
        const _rejectedCallback = this._rejectedCallbacks[index];
        // 保持成功回调的队列与失败回调的队列一致
        this._fulfilledCallbacks.shift();
        if (typeof _rejectedCallback === "function") {
          this.result = _rejectedCallback(this.result);
          // 异步场景下第一个 then 中失败的回调的返会结果，传递给下一个 then 中成功的回调
          resolve.call(this, this.result);
          break;
        }
      }
    }
  }
  try {
    executor(resolve.bind(this), reject.bind(this));
  } catch (e) {
    // 执行函数报错时，直接抛出错误给 reject 函数
    reject(e);
  }
}
/**
 * promise then 属性
 * @param {Function} onFulfilled 成功的回调
 * @param {Function} onRejected 失败的回调
 */
Object.defineProperty(promise.prototype, "then", {
  value: function (onFulfilled, onRejected) {
    if (this.status === PENDING) {
      this._fulfilledCallbacks.push(onFulfilled);
      this._rejectedCallbacks.push(onRejected);
      return this;
    }
    if (this.status === FULFILLED) {
      this.result = onFulfilled(this.result);
      return new promise((resolve, reject) => {
        resolve(this.result);
      });
    }
    if (this.status === REJECTED) {
      if (typeof onRejected === "function") {
        this.result = onRejected(this.result);
        return new promise((resolve, reject) => {
          resolve(this.result);
        });
      } else {
        return new promise((resolve, reject) => {
          reject(this.result);
        });
      }
    }
  },
});

/**
 * promise catch 属性
 * @param {Function} onRejected 失败的回调
 */
Object.defineProperty(promise.prototype, "catch", {
  value: function (onRejected) {
    if (this.status === REJECTED) {
      onRejected(this.result);
    }
    if (this.status === PENDING) {
      this._rejectedCallbacks.push(onRejected);
    }
  },
});

```


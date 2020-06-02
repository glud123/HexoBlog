---
title: Reduce实现原理
date: 2020-03-16 22:17:09
tags: [前端，基础]
categories: es6

---

# Array.prototype.reduce实现原理

> [MDN 对 `Array.prototype.reduce`的描述](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

`reduce()`方法对数组中的每个元素执行一个用户自定义的`reducer`函数（升序执行），将其结果汇总为单个返回值。

## 语法

```javascript
array.reduce(callback(accumulator, currentValue[, index, array])[,initialValue])
```

## 参数

#### callback

执行数组中的每一项（如果没有提供`initialValue`则第一项除外），包含以下四个参数：

- accumulator

  累计器累计回调的返回值；它是上一次调用回调时返回的累积值，或`initialValue`

- currentValue

  数组中正在处理的元素

- index（可选）

  数组中正在处理的当前元素的索引，如果提供了`initialValue`，则起始索引为0，否则从索引1开始

- array（可选）

  调用`reduce()`的数组

#### initialValue (可选)

作为第一次调用`callback`函数时的第一个参数的值，如果没有提供初始值，则使用数组中第一个元素。在没有初始值的空数组上调用`reduce()`将会报错。

<!--more-->

## 返回值

函数累计处理的结果

## 分析

根据`Array.prototype.reduce`的语法、参数可以知道`reduce`是在数组对象原型上的属性，且调用时会接收两个参数，其中必要参数是个回调函数，可选参数的是此回调函数的初始值。

## 原理实现

首先，我们先在数组原型上添加`reduce`属性

```javascript
Object.defineProperty(Array.prototype, "reduce", {});
```

上方的代码只是简单的在`Array`原型上添加了`reduce`属性，接下来完善`reduce`属性方法

```javascript
Object.defineProperty(Array.prototype, "reduce", {
  value: function(callback, initialValue){
    let o = Object(this); // 调用 reduce 的数组
    let l = o.length >>> 0; // 数组长度
    let v = initialValue; // 累计值 默认赋值初始值
    let k = 0; // 默认下标
    if(!initialValue){ 
      // 不存在初始值时，取数组第一个元素
      v = o[k++];
    }
    // 正序遍历数组每一项
    while(k < l){
      if(k in o){
        // 调用自定义的 reducer ,并将结果返回累计值
        v = callback(v, o[k], k, o);
      }
      k++;
    }
    return v;
  }
});
```

上方代码我们已经实现了最基本的 `reduce`，但是当我们传了空数组并且没有赋初始值得话我们的程序就会报错了，接下来继续完善`reduce`

```javascript
Object.defineProperty(Array.prototype, "reduce", {
  value: function(callback){
    if(this === null){
      throw new TypeError("Array.prototype.reduce called on null or undefined");
    }
    if(typeof callback !== "function"){
      throw new TypeError("Callback must be a function");
    }
    let o = Object(this); // 调用 reduce 的数组
    let l = o.length >>> 0; // 数组长度 默认为0
    let k = 0; // 默认下标
    let v; // 累计值
    if(arguments.length >= 2){
      // 存在初始值时，取初始值
      v = arguments[1];
    }else{
      /**
       * 之所以这么判断，防止以下场景报错
       * let arr = new Array();
       * arr.length = 10;
       * arr.reduce((a, b)=>a + b);
       */
      while(k < l && !(k in o)){
        k++;
      }
      if(k >= l){
        throw new TypeError("Reduce of empty array whit no initial value");
      }
      // 不存在初始值时，取数组第一个元素
      v = o[k++];
    }
    while(k < l){
      if(k in o){
        // 调用自定义的 reducer ,并将结果返回累计值
        v = callback(v, o[k], k, o);
      }
      k++;
    }
    return v;
  }
});
```

上方代码就是 `Array.prototype.reduce`的Polyfill的实现。

## 
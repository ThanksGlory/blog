---
title: JavaScript 面试题
date: 2024-07-30 11:11:07
typora-root-url: ../
cover: /images/cover/scope.png
top_img: false
tags: JavaScript
categories:
  - JavaScript
  - 面试题
---

JavaScript面试题主要考察对JavaScript语言的基础知识、核心概念、常用API以及在实际开发中问题的解决能力。常见的面试题类型包括数据类型、类型转换、原型和原型链、作用域和闭包、事件循环、异步编程、ES6新特性等。

# JS高频面试题

## 实现函数call、apply、bind

call方法实现过程

```javascript
Function.prototype.call = function (context) {

  console.log('test call');
  /**
   * 将函数设为对象的属性
   * 注意：非严格模式下, 指定为 null 和 undefined 的 this 值会自动指向全局对象(浏览器中就是 window 对象)
   * 值为原始值(数字，字符串，布尔值)的 this 会指向该原始值的自动包装对象(用 Object() 转换
   **/
  context = context ? Object(context) : window;
  context.fn = this;

  // 处理剩余参数
  let args = [...arguments].slice(1);
  let result = context.fn(...args);

  // 删除该函数
  delete context.fn;

  // 返回函数的返回值
  return result;
};

const a = { a: 1, b: 2, }
function callTest() {
  return this.a + this.b;
}

const s1 = callTest.call(a);
console.log(s1); // 3
```

apply实现过程

```javascript
Function.prototype.apply = function (context) {
  context = context ? Object(context) : window;
  context.fn = this;

  var result;
  // 需要判断是否存储第二个参数
  // 如果存在，就将第二个参数展开
  if (arguments[1]) {
    result = context.fn(...arguments[1])
  } else {
    result = context.fn()
  }

  return result;
}
```

bind实现过程

> 1、获取调用bind的函数。
>
> 2、返回一个函数。
>
> 3、判断是否是通过new调用的函数。
>
> 4、改变this指向并返回返回函数的返回值。

```javascript
Function.prototype.bind = function (context, ...args) {
  // 获取调用bind的函数
  var fn = this;
  if (typeof fn !== 'function') throw new TypeError('this is not a function');
  return function (...rest) {
    // 判断是否是通过new调用的函数
    if (new.target) {
      return new fn(...args, ...rest)
    }
    // 改变this指向并返回返回函数的返回值
    return fn.apply(context, [...args, ...rest])
  }
}
```

## 实现new关键字

new的实现过程分四个步骤：

> 1、创建一个空对象
>
> 2、改变对象的原型
>
> 3、改变this指向
>
> 4、构造函数返回的是引用类型，那么new操作符无效，否则是生效的

```javascript
function _new(fn, ...args) {
  // 1、创建一个空对象
  // 2、改变对象的原型
  const obj = Object.create(fn.prototype);
  // 3、改变this指向
  const result = fn.apply(obj, args);
  // 4、构造函数返回的是引用类型，那么new操作符无效，否则是生效的
  return (result && typeof result === 'object') ? result : obj;
}
```

**tips**：`Object.create()` 静态方法以一个现有对象作为原型，创建一个新对象。

## 手写实现promise函数

实现思路：1、实现构造器。2、实现then方法。

> 实现构造器。
>
> 1、实现构造器中需实现resolve和reject，这两个方法的作用都是`修改promise的状态`和`设置promise结果`。
>
> 2、构造器中executor执行出现`同步报错则修改promise状态`，`异步报错无法拦截状态为初始状态`。
>
> 实现then方法
>
> 1、then方法返回一个新的promise。其中有四个方法需要存储到队列中`onFulfilled`，`onRejected`，`resolve`，`reject`。
>
> 2、实现一个run方法在状态不是`pending`的时候调用`runHandler`并传递存储的方法。
>
> 3、`runHandler`在微队列中实现：
>
> ​	1）、callback递的回调不是函数，则进行状态穿透（保持调用then方法的promis的状态）。
>
> ​	2）、callback递的回调是函数，执行callback。

```javascript
const PENDING = "pending";
const FULFILLED = "fulfilled";
const REJECTED = "rejected";

class MyPromise {

  #result = undefined;
  #state = PENDING;
  #handlers = [];

  constructor(executor) {

    const resolve = (data) => {
      this.#changeState(FULFILLED, data);
    }
    const reject = (err) => {
      this.#changeState(REJECTED, err);
    }

    // 捕获同步错误
    try {
      executor(resolve, reject)
    } catch (error) {
      reject(error)
    }
  }

  #changeState(state, result) {
    if (this.#state !== PENDING) return;
    this.#result = result;
    this.#state = state;
    this.#run();
  }

  #runHandler(callback, resolve, reject) {
    if (typeof callback !== "function") {
      // 传递的回调不是函数，则进行状态穿透。
      // 保持调用then方法的promis的状态，并将调用then方法的promis的结果返回
      queueMicrotask(() => {
        let settled = this.#state === FULFILLED ? resolve : reject;
        settled(this.#result);
      });
    } else {
      queueMicrotask(() => {
        try {
          const data = callback(this.#result);
          resolve(data);
        } catch (error) {
          reject(error);
        }
      });
    }
  }

  #run() {
    if (this.#state === PENDING) return;
    while (this.#handlers.length) {
      const { onFulfilled, onRejected, resolve, reject } = this.#handlers.shift()
      if (this.#state === FULFILLED) {
        this.#runHandler(onFulfilled, resolve, reject)
      } else {
        this.#runHandler(onRejected, resolve, reject)
      }
    }
  }

  then(onFulfilled, onRejected) {
    return new MyPromise((resolve, reject) => {
      this.#handlers.push({
        onFulfilled,
        onRejected,
        resolve,
        reject
      });
      this.#run();
    })
  }
}
```

## 数据类型和变量

JavaScript共有八种数据类型，分别是 `undefined`、`null`、`boolean`、`number`、`string`、`symbol`、`bigint`、`object`。其中七种基本类型，加一种引用类型。

其中 symbol 和 bigInt 是ES6 中新增的数据类型：

- symbol 代表创建后独一无二且不可变的数据类型，它主要是为了解决可能出现的全局变量冲突的问题。
- bigint是一种数字类型的数据，它可以表示任意精度格式的整数，使用 BigInt 可以安全地存储和操作大整数，即使这个数已经超出了 number 能够表示的安全整数范围。

这些数据可以分为原始数据类型和引用数据类型：

- 栈：原始数据类型（undefined、null、boolean、number、string、symbol、bigint）
- 堆：引用数据类型（对象、数组和函数）

> 1、JS 有哪些数据类型，如何判断这些数据类型 ？（腾讯、阿里、滴滴、货拉拉、百度、招银、字节）
>
> 2、 number 类型表示整数的最大范围（字节）
>
> 3、什么是变量提升 ？（腾讯、网易、小米）
>
> 4、typeof(NaN) 返回什么 ？（滴滴）
>
> 5、 typeof(null) 为什么返回的是 'object'（滴滴）
>
> 6、`null` 和 `undefined`的区别 ？（同花顺、滴滴）
>
> 7、console.log([] == `false`）的输出结果（同花顺）
>
> 8、== 和 === 的区别？（滴滴）
>
> 9、const、let、var 区别（叠纸、字节）
>
> 10、 const 定义的值一定是不能改变的吗？（滴滴）
>
> 11、 const 声明了数组，还能 push 元素吗，为什么？（阿里）
>
> 12、JS 获取字符串的第 N 个字符（网易）
>
> 13、const 声明生成对象的时候，如何使其不可更改（字节）
>
> 14、 这两种方式的区别 ？typeof 判断（字节）
>
> ```javascript
> const str1 = "abc";
> const str2 = new String("abc");
> ```
>
> 


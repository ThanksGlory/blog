---
title: Reflect
date: 2025-07-25 09:45:28
typora-root-url: ../
cover: /images/cover/reflect .png
top_img: false
tags: JavaScript
categories:
  - JavaScript
  - js基础
---
Reflect 是一个内置的对象，它提供拦截 JavaScript 操作的方法。这些方法与 proxy handler 的方法相同。Reflect 不是一个函数对象，因此它是不可构造的。与大多数全局对象不同 Reflect 并非一个构造函数，所以不能通过 new 运算符对其进行调用，或者将 Reflect 对象作为一个函数来调用。Reflect 的所有属性和方法都是静态的（就像 Math 对象）。

```javascript
const obj = {
  a: 1,
  b: 2,
  get c() {
    return this.a + this.b;
  }
};

/**
 * 直接返回拦截不到c里面被读取的属性 
 * this指向obj
 */
const p1 = new Proxy(obj, {
  get(target, key) {
    console.log('read', key); // read c
    return target[key]
  }
});

console.log(p1.c); // 3

/** 
 * 通过使用Reflect.get(target, key, c) 可同时拦截a，b的获取 
 * this指向p2代理对象
 */
const p2 = new Proxy(obj, {
  get(target, key, receiver) {
    console.log('read', key);  // read c read a read b
    return Reflect.get(target, key, receiver)
  }
});

console.log(p2.c); // 3
```


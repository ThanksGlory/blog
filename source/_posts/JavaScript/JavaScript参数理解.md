---
title: JavaScript参数理解（如果参数是引用对象）
date: 2022-01-26 16:19:00
typora-root-url: ../
cover: /images/cover/params.webp
top_img: false
tags: JavaScript
comments: false
categories:
  - JavaScript
---

### 一道JavaScript题引出的关于JavaScript的函数参数问题？

```js
var a = [1];
function f(a){
  a[100] = 3
  a = [1,2,3];
}
f(a)
console.log(a);
```

执行到 a[100] = 3这行代码的时候，a的值为(101) [1, empty × 99, 3] ，此时参数地址和全局变量地址一样，如下图：

![image-20220126170031286](/assets/image-20220126170031286.png)

当执行a = [1,2,3]，此时参数的地址发生变化如下图：

![image-20220126170442812](/assets/image-20220126170442812.png)

全局变量a的地址还是之前的地址（**0x200007a4**）其值为[1, empty × 99, 3]，但是f函数执行上下文中的a地址变成了**0x200007a6**，其值为[1, 2, 3]；所以最后在全局打印的时候打印的是[1, empty × 99, 3]；

**总结：<font color="red">参数变量只会影响函数作用域，而不会影响全局作用域<font color="blue">(引用类型在不改变引用的情况下会影响该变量)</font>，而且随着局部作用域的消失而消失；</font>**

### 参数类型

- 基本类型
- 引用类型
- 函数类型(该类型包含在引用类型中，因其特殊性将其单独拎出来)

#### 基本类型

基本类型存储在栈内存（stack）中的简单数据段，也就是说，它们的值直接存储在变量访问的位置，参数会直接将值接收到，从而不会影响传入的值。

```js
var a = 1;
function f(a){
  a = 2;
  console.log(a); // 打印的是2
}
f(a)
console.log(a); // 打印的是1
```

#### 引用类型

引用类型存储在堆内存（heap）中的对象，也就是说，存储在变量处的值是一个指针，指向堆中的对象，所以在参数中如果改变了该参数的引用，该参数的修改或者赋值就和原来传入进来的值没有任何关系了，这就是上面题所出现的问题。

```js
var a = [1];
function f(a){
  a[100] = 3
  a = [1,2,3];
  console.log(a); // 打印的是[1,2,3]
}
f(a)
console.log(a); // 打印的是[1, empty × 99, 3]
```

#### 函数类型

将函数作为参数进行值的传递，我们将这种方式称之为回调函数，关于回调函数后面会单独讲解一次。

### 默认参数值

**函数默认参数**允许在没有值或`undefined`被传入时使用默认形参。

```js
function multiply(a, b = 1) {
  return a * b;
}

console.log(multiply(5, 2));
// expected output: 10

console.log(multiply(5));
// expected output: 5
```

#### 语法

> function [name]([param1[ = defaultValue1 ][, ..., paramN[ = defaultValueN ]]]) {
>     statements
> }

#### 描述

JavaScript 中函数的参数默认是`undefined`。然而，在某些情况下可能需要设置一个不同的默认值。这是默认参数可以帮助的地方。

以前，一般设置默认参数的方法是在函数体测试参数是否为`undefined`，如果是的话就设置为默认的值。

下面的例子中，如果在调用`multiply`时，参数`b`的值没有提供，那么它的值就为`undefined`。如果直接执行`a * b`，函数会返回 `NaN`。

```js
function multiply(a, b) {
  return a * b;
}

multiply(5, 2); // 10
multiply(5);    // NaN !
```



### Arguments对象

**`arguments`** 是一个对应于传递给函数的参数的类数组对象，`arguments`对象不是一个 [`Array`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array) 。它类似于`Array`，但除了length属性和索引元素之外没有任何`Array`属性。

```js
function func1(a, b, c) {
  console.log(arguments[0]);
  // expected output: 1

  console.log(arguments[1]);
  // expected output: 2

  console.log(arguments[2]);
  // expected output: 3
}

func1(1, 2, 3);
```

> 注意：
>
> 1、如果您正在编写与 ES6 兼容的代码，则应首选使用剩余参数。
>
> 2、“类数组”意味着参数具有长度属性和从零开始索引的属性，但它没有数组的内置方法，如 forEach() 和 map()。 如要使用数组的方法需要将其转换成数组。

### 剩余参数

**剩余参数**语法允许我们将一个不定数量的参数表示为一个数组。

```js
function sum(...theArgs) {
  return theArgs.reduce((previous, current) => {
    return previous + current;
  });
}

console.log(sum(1, 2, 3));
// expected output: 6

console.log(sum(1, 2, 3, 4));
// expected output: 10
```

#### 语法

> function(a, b, ...theArgs) {
>   // ...
> }

#### 描述

如果函数的最后一个命名参数以`...`为前缀，则它将成为一个由剩余参数组成的真数组，其中从`0`（包括）到`theArgs.length`（排除）的元素由传递给函数的实际参数提供。

在上面的例子中，`theArgs`将收集该函数的第三个参数（因为第一个参数被映射到`a`，而第二个参数映射到`b`）和所有后续参数。

[剩余参数和 `arguments`对象的区别](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Rest_parameters#剩余参数和_arguments对象的区别)

剩余参数和 `arguments`对象之间的区别主要有三个：

- 剩余参数只包含那些没有对应形参的实参，而 `arguments` 对象包含了传给函数的所有实参。
- `arguments`对象不是一个真正的数组，而剩余参数是真正的 `Array`实例，也就是说你能够在它上面直接使用所有的数组方法，比如 `sort`，`map`，`forEach`或`pop`。
- `arguments`对象还有一些附加的属性 （如`callee`属性）。

### 解构剩余参数

剩余参数可以被解构，这意味着他们的数据可以被解包到不同的变量中。请参阅[解构赋值](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)。

```js
function f(...[a, b, c]) {
  return a + b + c;
}

f(1)          // NaN (b and c are undefined)
f(1, 2, 3)    // 6
f(1, 2, 3, 4) // 6 (the fourth parameter is not destructured)
```


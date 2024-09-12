---
title: Dart语言学习-基础类型-布尔 bool
date: 2022-08-24 23:03:48
typora-root-url: ../
cover: /images/cover/dart.png
top_img: false
tags: Flutter
comments: false
categories:
  - Flutter
  - dart
---

# 布尔

## 声明

为了代表布尔值，Dart 有一个名字为 bool 的类型。 只有两个对象是布尔类型的：true 和 false 所创建的对象， 这两个对象也都是编译时常量。

[bool](https://api.dartlang.org/stable/2.17.1/dart-core/bool-class.html)

```dart
bool a;
print(a);
```

## true 判断

只有 true 对象才被认为是 true。 所有其他的值都是 flase。

```dart
String name = 'ducafecat';
if(name) {
  print('this is name');
}
```

## assert 断言

```dart
var a = true;
assert(a);

var name = '';
assert(name.isEmpty);
assert(name.isNotEmpty);

var num = 0 / 0;
assert(num.isNaN);
```

> 注意： 断言只在检查模式下运行有效，如果在生产模式 运行，则断言不会执行。

## 逻辑运算符

### `&&` 逻辑与

```dart
bool a = true;
bool b = true;
assert(a && b);
```

### `||` 逻辑或

```dart
bool a = true;
bool b = false;
assert(a || b);
```

### `!` 逻辑非

```dart
bool a = true;
bool b = !a;
print(b);
```

## 关系运算符

### `==` 等于

```dart
if(a == b) {}
```

### `!=` 不等于

```dart
if(a != b) {}
```

### `>` 大于

```dart
if(a > b) {}
```

### `>=` 大于或等于

```dart
if(a >= b) {}
```

### `<` 小于

```dart
if(a < b) {}
```

### `<=` 小于或等于

```dart
if(a <= b) {}
```

---
title: Dart语言学习-基础类型-常量
date: 2022-08-24 22:19:37
typora-root-url: ../
cover: /images/cover/dart.png
top_img: false
tags: Flutter
comments: false
categories:
  - Flutter
  - dart
---

# 常量

## 相同点

### 类型声明可以省略

```dart
final String a = 'ducafecat';
final a = 'ducafecat';

const String a = 'ducafecat';
const a = 'ducafecat';
```

### 初始后不能再赋值

```dart
final a = 'ducafecat';
a = 'abc';

const a = 'ducafecat';
a = 'abc';
```

### 不能和 var 同时使用

```dart
final var a = 'ducafecat';
const var a = 'ducafecat';
```

## 不同点

### 需要确定的值

```dart
final dt = DateTime.now();

const dt = const DateTime.now();
```

### 不可变性可传递

```dart
final List ls = [11, 22, 33];
ls[1] = 44;

const List ls = [11, 22, 33];
ls[1] = 44;
```

### 内存中重复创建

```dart
final a1 = [11 , 22];
final a2 = [11 , 22];
print(identical(a1, a2));

const a1 = [11 , 22];
const a2 = [11 , 22];
print(identical(a1, a2));
```

> `identical` 通过比较两个引用的是否是同一个对象判断是否相等

## 使用场景

### final 成员变量初始

```dart
class PlaceholdWidget extends StatelessWidget {

  final String? assetImagePath;

  const PlaceholdWidget({
    Key? key,
    this.assetImagePath,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    ...
  }
}
```

### const 全局参数

```dart
  // 本地存储key
  static const storageFirstOpen = 'first_open';
  static const storageLanguageCode = 'language_code';
  static const storageThemeCode = 'theme_code';
  static const storageToken = 'token';
  static const storageProfile = 'profile';
```

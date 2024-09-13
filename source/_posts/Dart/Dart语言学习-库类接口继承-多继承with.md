---
title: Dart语言学习-库类接口继承-多继承 with
date: 2022-09-03 20:41:36
typora-root-url: ../
cover: /images/cover/dart.png
top_img: false
tags: Flutter

categories:
  - Flutter
  - dart
---

# 多继承

## 多继承 with

- 定义类

```dart
class Phone {
  void call() {
    print('Phone is calling...');
  }
}

class Android {
  void playStore() {
    print('Google play store');
  }
}

class Ios {
  void appleStore() {
    print('Apply store');
  }
}
```

- with 混入

```dart
class Xiaomi with Phone, Android, Ios {}
```

> 采用 `with ... , .... , ...` 方式 mixin 入多个类功能

- 执行

```dart
void main(List<String> args) {
  var p = Xiaomi();
  p.call();
  p.playStore();
  p.appleStore();
}

Phone is calling...
Google play store
Apply store
```

## 函数重名冲突

- Android Ios 加入 call 函数

```dart
class Android {
  void playStore() {
    print('Google play store');
  }

  void call() {
    print('Android phone is calling...');
  }
}

class Ios {
  void appleStore() {
    print('Apply store');
  }

  void call() {
    print('Ios phone is calling...');
  }
}
```

- 执行

```dart
void main(List<String> args) {
  var p = Xiaomi();
  p.call();
  p.playStore();
  p.appleStore();
}

Ios phone is calling...
Google play store
Apply store
```

> 可以发现后面的覆盖了前面的内容

## mixin 不能构造函数

- 加入构造函数

```dart
class Android {
  Android();
  ...
}

The class 'Android' can't be used as a mixin because it declares a constructor.
```

- 加入 mixin 关键字 限定

```dart
mixin Android {
  // mixin 中不能定义 constructor
  ...
}
```

## mixin on 限定条件

- 关键字 on 限定 Phone

```dart
mixin Android on Phone {
  void playStore() {
    print('Google play store');
  }

  @override
  void call() {
    super.call();
    print('Android phone is calling...');
  }
}
```

- with 混入时候，必须先 Phone 才行

错误

```dart
class Xiaomi with Android {}

'Android' can't be mixed onto 'Object' because 'Object' doesn't implement 'Phone'.
Try extending the class 'Android'.
```

正确

```dart
class Xiaomi with Phone,Android {}
```

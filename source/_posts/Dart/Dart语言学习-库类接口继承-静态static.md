---
title: Dart语言学习-库类接口继承-静态 static
date: 2022-08-28 22:30:39
typora-root-url: ../
cover: /images/cover/dart.png
top_img: false
tags: Flutter
comments: false
categories:
  - Flutter
  - dart
---

# static

## 静态变量

### static 定义

声明

```dart
class People {
  static String name = 'ducafecat';
}
```

调用

静态变量可以通过外部直接访问,不需要将类实例化

```dart
print(People.name);

ducafecat
```

### 函数内部访问

实例化后的类也可以访问该静态变量

声明

```dart
class People {
  static String name = 'ducafecat';
  void show() {
    print(name);
  }
}
```

调用

```dart
People().show();

ducafecat
```

## 静态方法

静态方法可以通过外部直接访问

声明

```dart
class People {
  static String name = 'ducafecat';
  static void printName() {
    print(name);
  }
}
```

调用

```dart
People.printName();

ducafecat
```

## 业务场景

EasyLoading 用大量的 static 来方便方法调用

![20220601095740](/assets/20220601095740.png)

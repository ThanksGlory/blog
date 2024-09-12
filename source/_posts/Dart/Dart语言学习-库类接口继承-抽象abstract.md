---
title: 库类接口继承-抽象 abstract
date: 2022-09-03 15:28:41
typora-root-url: ../
cover: /images/cover/dart.png
top_img: false
tags: Flutter
comments: false
categories:
  - Flutter
  - dart
---

# 抽象

## abstract 类、函数、成员

- 普通类前加 abstract

```dart
abstract class Person {
  String name = 'ducafecat';
  void printName() {
    print(name);
  }
}
```

## 不能直接 new 实例化

```dart
var p = Person();
p.printName();

Abstract classes can't be instantiated.
Try creating an instance of a concrete subtype.
```

## 继承方式使用

```dart
class Teacher extends Person {
}

var p = Teacher();
p.printName();

ducafecat
```

## 接口方式使用

定义

```dart
abstract class Person {
  String name = '';
  void printName();
}

class Teacher implements Person {
  @override
  String name;

  Teacher(this.name);

  @override
  void printName() {
    print('Teacher: $name');
  }
}
```

实例

```dart
var p = Teacher("ducafecat");
p.printName();

ducafecat
```

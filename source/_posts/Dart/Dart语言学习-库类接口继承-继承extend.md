---
title: Dart语言学习-库类接口继承-继承 extend
date: 2022-09-03 20:10:19
typora-root-url: ../
cover: /images/cover/dart.png
top_img: false
tags: Flutter

categories:
  - Flutter
  - dart
---

# 继承

## 实现继承

```dart
class Phone {
  void startup() {
    print('开机');
  }
  void shutdown() {
    print('关机');
  }
}

class AndroidPhone extends Phone {
}

void main() {
  var p = AndroidPhone();
  p.startup();
  p.shutdown();
}

开机
关机
```

## 父类调用

```dart
class Phone {
  void startup() {
    print('开机');
  }

  void shutdown() {
    print('关机');
  }
}

class AndroidPhone extends Phone {
  @override
  void startup() {
    super.startup();
    print('AndroidPhone 开机');
  }
}

void main() {
  var p = AndroidPhone();
  p.startup();
}

开机
AndroidPhone 开机
```

> super 对象可以访问父类

## 调用父类构造

```dart
class Mobile {
  int number;
  Mobile(this.number);
  void showNumber() {
    print('010-$number');
  }
}

class AndroidPhone extends Mobile {
  AndroidPhone(int number) : super(number);
}

void main() {
  var p = AndroidPhone(12345678);
  p.showNumber();
}

010-12345678
```

> 可调用父类的 构造函数

## 重写超类函数

```dart
class Mobile {
  int number;
  Mobile(this.number);
}

class AndroidPhone extends Mobile {
  AndroidPhone(int number) : super(number);

  @override
  void noSuchMethod(Invocation mirror) {
    print('被重写 noSuchMethod');
  }
}

void main() {
  dynamic p = AndroidPhone(12345678);
  p.showNumber111();
}

被重写 noSuchMethod
```

> 在重写的函数上加修饰符 `@override`

## 继承抽象类的问题

```dart
abstract class IPhone {
  void startup() {
    print('开机');
  }

  void shutdown();
}

class AndroidPhone extends IPhone {
  @override
  void startup() {
    super.startup();
    print('AndroidPhone 开机');
  }

  @override
  void shutdown() {
    print('AndroidPhone 关机');
  }
}

void main() {
  var p = AndroidPhone();
  p.startup();
  p.shutdown();
}

开机
AndroidPhone 开机
AndroidPhone 关机
```

> 抽象类中只定义抽象函数，实例化访问会报错

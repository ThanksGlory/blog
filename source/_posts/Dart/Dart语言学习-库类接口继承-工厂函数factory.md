---
title: Dart语言学习-库类接口继承-工厂函数 factory
date: 2022-09-03 20:28:08
typora-root-url: ../
cover: /images/cover/dart.png
top_img: false
tags: Flutter

categories:
  - Flutter
  - dart
---

# 工厂函数

## 调用子类

```dart
abstract class Phone {
  void call();
  factory Phone(String type) {
    switch (type) {
      case "android":
        return Android();
      case "ios":
        return Ios();
      default:
        throw "The '$type' is not an animal";
    }
  }
}

class Android implements Phone {
  @override
  void call() {
    print('Android Calling...');
  }
}

class Ios implements Phone {
  @override
  void call() {
    print('Ios Calling...');
  }
}

void main() {
  var android = Phone('android');
  var ios = Phone('ios');
  android.call();
  ios.call();
}
```

## 单例模式

```dart
class Phone {
  static final Phone _single = Phone._internal();
  Phone._internal();

  factory Phone() {
    return _single;
  }

  void call() {
    print('Calling...');
  }
}

void main() {
  var p1 = Phone();
  var p2 = Phone();
  print(identical(p1, p2));

  Phone().call();
}
```

## 减少重复实例对象

```dart
class Phone {
  int _number;
  Phone(this._number);

  factory Phone.fromJson(Map<String, dynamic> json) =>
      Phone(json['number'] as int);

  void call() {
    print('Calling $_number...');
  }
}

void main() {
  var p = Phone.fromJson({"number": 911});
  p.call();
}
```

> 如果不用工厂函数，就要用类静态方法，这样会有多余的实例对象

---
title: Dart语言学习-进阶使用-泛型 generices
date: 2022-09-03 21:10:09
typora-root-url: ../
cover: /images/cover/dart.png
top_img: false
tags: Flutter
comments: false
categories:
  - Flutter
  - dart
---

# 泛型 generics

## 泛型使用

```dart
main(List<String> args) {
  var l = <String>[];
  l.add('aaa');
  l.add('bbb');
  l.add('ccc');
  print(l);

  var m = <int, String>{};
  m[1] = 'aaaa';
  m[2] = 'bbbb';
  m[3] = 'cccc';
  print(m);
}

[aaa, bbb, ccc]
{1: aaaa, 2: bbbb, 3: cccc}
```

> 容器对象，在创建对象时都可以定义泛型类型。

## 泛型函数

```dart
K addCache<K, V>(K key, V val) {
  print('$key -> $val');
  return val;
}

main(List<String> args) {
  var key = addCache('url', 'https://wiki.doucafecat.tech');
  print(key);
}

url -> https://wiki.doucafecat.tech
https://wiki.doucafecat.tech
```

## 构造函数泛型

```dart
class Phone<T> {
  final T mobileNumber;
  Phone(this.mobileNumber);
}

main(List<String> args) {
  var p = Phone<String>('ducafecat');
  print(p.mobileNumber);
}

ducafecat
```

> 这是大多数情况下使用泛型的场景，在一个类的构造函数中

## 泛型限制

- 定义

```dart
class AndroidPhone {
  void startup() {
    print('Android Phone 开机');
  }
}

class Phone<T extends AndroidPhone> {
  final T mobile;
  Phone(this.mobile);
}
```

- 实例

```dart
main(List<String> args) {
  var ad = AndroidPhone();
  var p = Phone<AndroidPhone>(ad);
  p.mobile.startup();
}

Android Phone 开机
```

> 通过 extends 关键字 可以限定你可以泛型使用的类型

---
title: Dart语言学习-库类接口继承-库 lib
date: 2022-09-03 20:52:44
typora-root-url: ../
cover: /images/cover/dart.png
top_img: false
tags: Flutter

categories:
  - Flutter
  - dart
---

# 库 lib

## 导入核心库

```dart
import 'dart:io';

void main() {
  var f = new File('README.md');
  var content = f.readAsStringSync();
  print(content);
}

# Dart 语言学习示例
```

## 导入第三方库

- 编写 `pubspec.yaml`

```yaml
dependencies:
  dio: ^4.0.6
```

- 执行拉取包命令

```shell
$ dart pub get
```

- 程序调用

```dart
import 'package:dio/dio.dart';

void main() async {
  Dio dio = Dio();
  Response<String> response =
      await dio.get("https://wpapi.ducafecat.tech/products/categories");
  print(response.data);
}

[{"id":34,"name":"Bag","slug":"bag","parent":0,"description":" ......
```

## 导入自己的 git 仓库

- 编写 `pubspec.yaml`

```dart
dependencies:
  uuid:
    git:
      url: https://github.com/Daegalus/dart-uuid
      ref: master
```

- 执行拉取包命令

```dart
$ dart pub get
```

> 从 master 分支拉取

## 导入类文件

- phone.dart

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

- 实例

```dart
import 'package:dart_learn/phone.dart';

void main() {
  var p = Phone();
  p.call();
}

Phone is calling...
```

## 前缀&别名

```dart
import 'package:dart_learn/phone.dart' as pp;

void main() {
  var p = pp.Android();
  p.call();
  p.playStore();
}

Android phone is calling...
Google play store
```

## 筛选包内容

```dart
import 'package:dart_learn/phone.dart' show Ios;

void main() {
  var p = Ios();
  p.call();
  p.appleStore();
}

Ios phone is calling...
Apply store
```

> `hide` 筛掉某几个包 `show` 只使用某几个包

## 延迟载入

```dart
import 'package:dart_learn/phone.dart' deferred as pp;

Future<void> main() async {
  await pp.loadLibrary();
  var p = pp.Android();
  p.call();
  p.playStore();
}

Android phone is calling...
Google play store
```

> `loadLibrary()` 方式在需要的时候载入包 可提高程序启动速度 用在不常使用的功能 用在载入时间过长的包

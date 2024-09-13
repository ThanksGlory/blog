---
title: Dart语言学习-类型定义 typedef
date: 2022-09-03 22:19:18
typora-root-url: ../
cover: /images/cover/dart.png
top_img: false
tags: Flutter

categories:
  - Flutter
  - dart
---

# 类型定义 typedef

- 显示这个函数的输入输出
- 简化常用函数、类型定义

## typedef 定义使用

- 采用 `typedef`

```dart
typedef MyPrint = void Function(String val);

class PrintClass {
  MyPrint print;
  PrintClass(this.print);
}

main() {
  PrintClass coll = PrintClass((String val) => print(val));
  coll.print('hello world');
}

hello world
```

## 未采用 typedef

```dart
class PrintClass {
  void Function(String val) print;
  PrintClass(this.print);
}

main() {
  PrintClass coll = PrintClass((String val) => print(val));
  coll.print('hello world');
}

hello world
```

## 简化常用类型定义

- 定义

```dart
typedef MapStringAny = Map<String, dynamic>;
typedef MapAnyString = Map<dynamic, String>;
typedef MapStringString = Map<String, String>;
typedef MapStringDouble = Map<String, double>;
typedef MapDoubleString = Map<double, String>;
typedef MapDoubleDouble = Map<double, double>;
typedef MapIntInt = Map<int, int>;
typedef MapIntDouble = Map<int, double>;

typedef ListString = List<String>;
typedef ListInt = List<int>;
typedef ListDouble = List<double>;
typedef ListAny = List<dynamic>;
```

- 使用

```dart
main() {
  ListString p = [];
  p.add('a');
  p.add('b');
  p.add('c');

  MapStringAny m = {};
  m['a'] = 1;
  m['b'] = 2;
  m['c'] = 3;
}
```

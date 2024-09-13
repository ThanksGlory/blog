---
title: Dart语言学习-空安全 null safe
date: 2022-09-03 22:28:39
typora-root-url: ../
cover: /images/cover/dart.png
top_img: false
tags: Flutter

categories:
  - Flutter
  - dart
---

# 空安全

- 减少数据异常错误
- 提高程序性能

## 默认不可空

```dart
String title = 'ducafecat';
```

## type? 可空

```dart
String? title = null;
```

## value! 值保证不为空，主观上

```dart
String? title = 'ducafecat';
String newTitle = title!;
```

## value?. 不为空才执行

```dart
String? title = 'ducafecat';
bool isEmpty = title?.isEmpty();
```

## value?? 如果空执行

```dart
String? title = 'ducafecat';
String newTitle = title ?? 'cat';
```

## late 声明

延迟加载修饰符

声明一个不可空的变量，并在声明后初始化。

```dart
late String description;

void main() {
  description = 'Feijoada!';
  print(description);
}
```

## late 类成员延迟初始

```dart
class WPHttpService extends GetxService {
  late final Dio _dio;

  @override
  void onInit() {
    ...

    _dio = Dio(options);

    ...
  }
```

> 加上 `late` 后就可以不用构造函数的时候初始化了

## List、泛型

```dart
  List<String?>? l;
  l = [];
  l.add(null);
  l.add('a');
  print(l);
```

| 类型           | 集合是否可空 | 数据项是否可空 |
| -------------- | ------------ | -------------- |
| List<String>   | no           | no             |
| List<String>?  | yes          | no             |
| List<String?>  | no           | yes            |
| List<String?>? | yes          | yes            |

## Map

```dart
  Map<String, String?>? m;
  m = {};
  m['a'] = 'b';
  m['b'] = null;
  print(m);
```

| 类型               | 集合是否可空 | 数据项是否可空 |
| ------------------ | ------------ | -------------- |
| Map<String, int>   | no           | no*            |
| Map<String, int>?  | yes          | no*            |
| Map<String, int?>  | no           | yes            |
| Map<String, int?>? | yes          | yes            |

> `*` 可能返回空

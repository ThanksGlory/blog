---
title: Dart语言学习-集合枚举-列表 List
date: 2022-08-28 15:29:22
typora-root-url: ../
cover: /images/cover/dart.png
top_img: false
tags: Flutter

categories:
  - Flutter
  - dart
---
# 列表

## 初始

List 是一个有序列表

```dart
var l = [1, 2, 3];
print(l);

[1, 2, 3]
```

## 声明

### 自动

```dart
List<int> l = [];
l
..add(1)
..add(2)
..add(3);
print(l);

[1, 2, 3]
```

### 定长

```dart
  var l = List<int>.filled(3, 0);
  l[0] = 1;
  l[1] = 2;
  l[2] = 3;
  print(l);

  [1, 2, 3]
```

### 生成数据

```dart
  List<int> l = List.generate(10, (index) {
    return index;
  });

  print(l);

  [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## 属性

| 名称       | 说明         |
| ---------- | ------------ |
| isEmpty    | 是否为空     |
| isNotEmpty | 是否不为空   |
| first      | 第一个对象   |
| last       | 最后一个对象 |
| length     | 个数         |
| reversed   | 反转         |

```dart
var l = [1, 2, 3];
print(l.first);
print(l.last);
print(l.length);
print(l.isEmpty);
print(l.isNotEmpty);
print(l.reversed);

1
3
3
false
true
(3, 2, 1)
```

## 方法

| 名称       | 说明         |
| ---------- | ------------ |
| add        | 添加         |
| addAll     | 添加多个     |
| insert     | 插入         |
| insertAll  | 插入多个     |
| indexOf    | 查询         |
| indexWhere | 按条件查询   |
| remove     | 删除         |
| removeAt   | 按位置删除   |
| fillRange  | 按区间填充   |
| getRange   | 按区间获取   |
| shuffle    | 随机变换顺序 |
| sort       | 排序         |
| sublist    | 创建子       |

### 添加

```dart
List<int> l = [];

l
  ..add(1)
  ..addAll([2, 3, 4, 5])
  ..insert(0, 6)
  ..insertAll(6, [6, 6])
  ;

print(l);

[6, 1, 2, 3, 4, 5, 6, 6]
```

### 查询

```dart
var l = [6, 1, 2, 3, 4, 5, 6, 6];

print(l.indexOf(5));
print(l.indexWhere((it) => it == 4));

5
4
```

### 删除

```dart
var l = [6, 1, 2, 3, 4, 5, 6, 6];

l.remove(6);
print(l);
l.removeAt(5);
print(l);

[1, 2, 3, 4, 5, 6, 6]
[1, 2, 3, 4, 5, 6]
```

### Range

```dart
var l = [6, 1, 2, 3, 4, 5, 6, 6];

l.fillRange(0, 3, 9);
print(l.getRange(0, 5));

(9, 9, 9, 3, 4)
```

### 洗牌

```dart
var l = [6, 1, 2, 3, 4, 5, 6, 6];

l.shuffle();
print(l);
l.shuffle();
print(l);

[2, 5, 4, 6, 6, 3, 1, 6]
[6, 4, 6, 5, 2, 3, 6, 1]
```

### 排序

- 数字

```dart
var l = [6, 1, 2, 3, 4, 5, 6, 6];

l.sort();
print(l);

[1, 2, 3, 4, 5, 6, 6, 6]
```

- 日期

```dart
List<DateTime> dtList = [];
dtList.addAll([
  DateTime.now(),
  DateTime.now().add(new Duration(days: -12)),
  DateTime.now().add(new Duration(days: -2))
  ]);
print(dtList);

dtList.sort((a, b) => a.compareTo(b));
print(dtList);

9
[2022-05-29 16:43:17.802, 2022-05-17 16:43:17.802, 2022-05-27 16:43:17.802]
[2022-05-17 16:43:17.802, 2022-05-27 16:43:17.802, 2022-05-29 16:43:17.802]
```

### 复制子列表

```dart
var l = [6, 1, 2, 3, 4, 5, 6, 6];

var l2 = l.sublist(1,4);
print(l2);

[1, 2, 3]
```

### 操作符

| 名称 | 说明 |
| ---- | ---- |
| +    | 连接 |
| []   | 取值 |
| []=  | 赋值 |

```dart
var l1 = [1, 2, 3];
var l2 = [4, 5, 6];
print(l1 + l2);
l1[2] = 9;
print(l1[2]);

[1, 2, 3, 4, 5, 6]
9
```

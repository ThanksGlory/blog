---
title: Dart语言学习-基础类型-日期时间 Datetime
date: 2022-08-24 23:41:42
typora-root-url: ../
cover: /images/cover/dart.png
top_img: false
tags: Flutter
comments: false
categories:
  - Flutter
  - dart
---

# 日期时间

## 声明

- 当前时间

```dart
var now = new DateTime.now();
print(now);

2022-05-28 20:04:43.607
```

- 指定年月日

```dart
var d = new DateTime(2018, 10, 10, 9, 30);
print(d);

2018-10-10 09:30:00.000
```

## 创建时间 UTC

- [UTC 协调世界时](https://zh.wikipedia.org/wiki/协调世界时)
- [原子时](https://zh.wikipedia.org/wiki/原子时)
- [原子钟](https://zh.wikipedia.org/wiki/原子鐘)
- 时区表

![20220528200356](/assets/20220528200356.png)

- 创建 utc 时间

```dart
var d = new DateTime.utc(2018, 10, 10, 9, 30);
print(d);

2018-10-10 09:30:00.000Z
```

## 解析时间 IOS 8601

- [ISO 8601](https://zh.wikipedia.org/wiki/ISO_8601)
- [时区](https://zh.wikipedia.org/wiki/时区)
- [时区列表](https://zh.wikipedia.org/wiki/时区列表)
- 如果时间在零时区

```dart
var d1 = DateTime.parse('2018-10-10 09:30:30Z');
print(d1);

2018-10-10 09:30:30.000Z
var d2 = DateTime.parse('2018-10-10 09:30:30+0800');
print(d2);

2018-10-10 01:30:30.000Z
```

## 时间增减量

```dart
var d1 = DateTime.now();
print(d1);
print(d1.add(new Duration(minutes: 5)));
print(d1.add(new Duration(minutes: -5)));

2022-05-28 22:09:12.805
2022-05-28 22:14:12.805
2022-05-28 22:04:12.805
```

## 比较时间

```dart
var d1 = new DateTime(2018, 10, 1);
var d2 = new DateTime(2018, 10, 10);
print(d1.isAfter(d2));
print(d1.isBefore(d2));

false
true
var d1 = DateTime.now();
var d2 = d1.add(new Duration(milliseconds: 30));
print(d1.isAtSameMomentAs(d2));

false
```

## 时间差

```dart
var d1 = new DateTime(2018, 10, 1);
var d2 = new DateTime(2018, 10, 10);
var difference = d1.difference(d2);
print([difference.inDays, difference.inHours]);

[-9, -216]
```

## 时间戳

- [公元](https://zh.wikipedia.org/wiki/公元)

```dart
var now = new DateTime.now();
print(now.millisecondsSinceEpoch);
print(now.microsecondsSinceEpoch);

1653747090687
1653747090687000
```

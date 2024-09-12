---
title: Dart语言学习-扩展 extension
date: 2022-09-03 23:36:05
typora-root-url: ../
cover: /images/cover/dart.png
top_img: false
tags: Flutter
comments: false
categories:
  - Flutter
  - dart
---

# 扩展 extension

extension 本质上还是和继承一样扩展了方法。

但这是一种简洁优雅的方式，你可以想想之前继承的繁琐。

## 示例 扩展日期时间

- 加入依赖包 pubspec.yaml

```yaml
dependencies:
  intl: ^0.17.0
```

- 编写扩展类 ExDateTime

```dart
import 'package:intl/intl.dart';

extension ExDateTime on DateTime {
  /// 方法，格式化日期 yyyy-MM-dd
  String toDateString({String format = 'yyyy-MM-dd'}) =>
      DateFormat(format).format(this);

  // 属性
  int get nowTicket => this.microsecondsSinceEpoch;
}

main() {
  var now = DateTime.now();

  print(now.toDateString());
  print(now.nowTicket);
}
```

> 我们给可以扩展类加个前缀 `Ex` 这样一看就知道是扩展

## 业务场景

- 功能函数

![20220604162911](/assets/20220604162911.png)

- 视图组件

![20220604163016](/assets/20220604163016.png)

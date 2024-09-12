---
title: Dart语言学习-库类接口继承-类 get set
date: 2022-08-28 20:36:43
typora-root-url: ../
cover: /images/cover/dart.png
top_img: false
tags: Flutter
comments: false
categories:
  - Flutter
  - dart
---

# get set

## 定义、使用 get set

定义

```dart
class People {
  String? _name;

  People();

  set name(String value) {
    _name = value;
  }

  String get name {
    return 'people is $_name';
  }
}
```

使用

```dart
var p = People();
p.name = 'ducafecat';
print(p.name);

people is ducafecat
```

## 简化 get set

```dart
class People {
  String? _name;

  People();

  set name(String value) => _name = value;

  String get name => 'people is $_name';
}
```

## 业务场景

购物车

```dart
  /// 商品数量
  int get lineItemsCount => lineItems.length;

  /// 运费
  double get shipping => 0;

  /// 折扣
  double get discount =>
      lineCoupons.fold<double>(0, (double previousValue, CouponsModel element) {
        return previousValue + (double.parse(element.amount ?? "0"));
      });

  /// 商品合计价格
  double get totalItemsPrice =>
      lineItems.fold<double>(0, (double previousValue, LineItem element) {
        return previousValue + double.parse(element.total ?? "0");
      });
```

> 以前可能会写个方法 `getXXX()` 当然也适用于赋值操作

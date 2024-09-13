---
title: Flutter-基础特性-松约束
date: 2022-09-05 16:34:16
typora-root-url: ../
cover: /images/cover/flutter.png
top_img: false
tags: Flutter

categories:
  - Flutter
  - flutter基础
---

# 松约束

## Column 宽度等于子元素最大宽度

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(build());
}

Widget build() {
  return MaterialApp(
    home: Scaffold(
      body: Column(
        children: const [
          Text("aaaaaaaaaaaaaaaaaaaaaa"),
          Text("aaaaaaaaaaa"),
        ],
      ),
    ),
  );
}
```

![image-20220628083227210](/assets/image-20220628083227210.png)

## Container 紧包裹子元素

- Scaffold 填充了整个屏幕，Container 包裹了 Column

```dart
class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: Container(
          color: Colors.amber,
          child: Column(
            children: const [
              Text("aaaaaaaaaaaaaaaa"),
              Text("bbbbbbbbb"),
            ],
          ),
        ),
      ),
    );
  }
}
```

![image-20220617212555636](/assets/image-20220617212555636.png)

- Container 的宽高随着 Column 一起变化 `w = 124.0`，紧包裹

![image-20220617213030796](/assets/image-20220617213030796.png)

## 松约束定义

当一个 widget 告诉其子级可以比自身更小的话， 我们通常称这个 widget 对其子级使用 **宽松约束（loose）**。

- Scaffold 对 Column 的约束是宽高屏幕宽度内即可

![image-20220617213257446](/assets/image-20220617213257446.png)

- Column 的宽度按 Text 最大宽度为准

![image-20220617213448521](/assets/image-20220617213448521.png)

## 参考

- https://api.flutter.dev/flutter/rendering/BoxConstraints-class.html

---
title: Flutter-基础特性-布局约束规则
date: 2022-09-05 16:06:21
typora-root-url: ../
cover: /images/cover/flutter.png
top_img: false
tags: Flutter
comments: false
categories:
  - Flutter
  - flutter基础
---

# 布局约束规则

## 让子元素竟可能的大，撑满父元素

- 在 main 函数中，直接创建 Container 显示

```dart
void main(List<String> args) {
  runApp(build());
}

Widget build() {
  return Container(
    width: 200,
    height: 200,
    color: Colors.amber,
  );
}
```

![2021-12-26-22-22-07](/assets/2021-12-26-22-22-07.png)

## 确认位置后，按子元素大小显示

- 用 Center 包裹 Container

```dart
void main(List<String> args) {
  runApp(build());
}

Widget build() {
  return Center(
    child: Container(
      width: 200,
      height: 200,
      color: Colors.amber,
    ),
  );
}
```

![2021-12-26-22-23-37](/assets/2021-12-26-22-23-37.png)

## 核心规则：Constraints go down. Sizes go up. Positions are set by parents.

- 上层 widget 向下层 widget 传递约束条件。
- 下层 widget 向上层 widget 传递大小信息。
- 上层 widget 决定下层 widget 的位置。

![2021-12-26-22-23-37](/assets/2021-12-26-22-23-37-1662365609905-3.png)

- 我们写3个文字组件纵向排列

```dart
Widget _buildScaffold() {
  return Scaffold(
    body: Column(
      children: const <Widget>[
        Text("aaaaaa"),
        Text("bbbbb"),
        Text("cccc"),
      ],
    ),
  );
}
```

![2021-12-26-22-23-37](/assets/2021-12-26-22-23-37-1662365633793-5.png)

- 宽度 `0.0 <= w <= 303.0` , 高度 `0.0 <= h <= 523.0` ，就是上层传下来的约束

![2021-12-26-22-23-37](/assets/2021-12-26-22-23-37-1662365657812-7.png)

- 宽度 `w=32.0` , 高度 `h=16.0` 就是组件向上层传递的大小信息

![2021-12-26-22-23-37](/assets/2021-12-26-22-23-37-1662365686551-9.png)

- 元素左边 `w=7.5`，右边 `w=7.5`，就是上层决定下层的组件位置

![2021-12-26-22-23-37](/assets/2021-12-26-22-23-37-1662365718743-11.png)

## 参考

- https://medium.com/flutter-community/flutter-the-advanced-layout-rule-even-beginners-must-know-edc9516d1a2
- https://juejin.cn/post/6846687593745088526
- https://docs.flutter.dev/development/ui/layout/constraints

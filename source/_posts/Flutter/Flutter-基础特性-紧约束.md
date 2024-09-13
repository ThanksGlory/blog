---
title: Flutter-基础特性-紧约束
date: 2022-09-05 16:45:28
typora-root-url: ../
cover: /images/cover/flutter.png
top_img: false
tags: Flutter

categories:
  - Flutter
  - flutter基础
---

# 紧约束

## ConstrainedBox 约束组件

- `constraints` 通过 `maxWidth` `maxHeight` ，来设置子组件最大约束

```dart
void main() {
  runApp(build());
}

Widget build() {
  return MaterialApp(
    home: Scaffold(
      body: Center(
        child: ConstrainedBox(
          constraints: const BoxConstraints(
            maxWidth: 200,
            maxHeight: 200,
          ),
          child: Container(
            color: Colors.amber,
            width: 50,
            height: 50,
          ),
        ),
      ),
    ),
  );
}
```

显示了一个 50 x 50 的 Container

![image-20220617214549883](/assets/image-20220617214549883.png)

- 我们加上 `minWidth` `minHeight`

```dart
void main() {
  runApp(build());
}

Widget build() {
  return MaterialApp(
    home: Scaffold(
      body: Center(
        child: ConstrainedBox(
          constraints: const BoxConstraints(
            minWidth: 100,
            minHeight: 100,
            maxWidth: 200,
            maxHeight: 200,
          ),
          child: Container(
            color: Colors.amber,
            width: 10,
            height: 10,
          ),
        ),
      ),
    ),
  );
}
```

这时候 尺寸强制被设置为了 100 * 100

![image-20220617214913498](/assets/image-20220617214913498.png)

- 查看约束情况

![image-20220617215049479](/assets/image-20220617215049479.png)

## 紧约束定义

它的最大/最小宽度是一致的，高度也一样。

- 通过 BoxConstraints.tight 可以设置紧约束

```dart
void main() {
  runApp(build());
}

Widget build() {
  return MaterialApp(
    home: Scaffold(
      body: Center(
        child: ConstrainedBox(
          constraints: BoxConstraints.tight(const Size(100, 100)),
          child: Container(
            color: Colors.amber,
            width: 10,
            height: 10,
          ),
        ),
      ),
    ),
  );
}
```

![image-20220617223634120](/assets/image-20220617223634120.png)

- 宽高约束都被设置成了 100

![image-20220617225309360](/assets/image-20220617225309360.png)

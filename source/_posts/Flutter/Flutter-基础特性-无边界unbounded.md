---
title: Flutter-基础特性-无边界 unbounded
date: 2022-09-05 16:55:57
typora-root-url: ../
cover: /images/cover/flutter.png
top_img: false
tags: Flutter
comments: false
categories:
  - Flutter
  - flutter基础
---

# 无边界 unbounded

## UnconstrainedBox 不受约束

- `UnconstrainedBox` 包裹内部的 Container 10*10 可以不受约束自己控制大小

```dart
class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: ConstrainedBox(
          constraints: const BoxConstraints(
            minWidth: 100,
            minHeight: 100,
            maxWidth: 300,
            maxHeight: 300,
          ),
          child: UnconstrainedBox(
            child: Container(
              width: 10,
              height: 10,
              color: Colors.blue,
            ),
          ),
        ),
      ),
      debugShowCheckedModeBanner: false,
    );
  }
}
```

- 显示了一个 `10*10` 的正方形，没有收到 `ConstrainedBox` 的影响

![image-20220617230952463](/assets/image-20220617230952463.png)

- 约束查看 Container 10*10，父级约束 最小宽高 100

![image-20220617231111345](/assets/image-20220617231111345.png)

## unbounded 组件

- `Row` `Column` `ListView` 这种组件 属于 `unbounded`

```dart
class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: Column(
          children: const [
            FlutterLogo(size: 50),
            FlutterLogo(size: 20),
            // Container(
            //   height: 2000,
            //   color: Colors.amber,
            // ),
          ],
        ),
      ),
      debugShowCheckedModeBanner: false,
    );
  }
}
```

![image-20220617230151965](/assets/image-20220617230151965.png)

- Column 垂直元素 unconstrained 没有约束，高度不限制

![image-20220617230033041](/assets/image-20220617230033041.png)

- 加入一个高度超出屏幕区域的 Container

```dart
class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: Column(
          children: [
            const FlutterLogo(size: 50),
            const FlutterLogo(size: 20),
            Container(
              height: 2000, // 设置了 2000 高
              color: Colors.amber,
            ),
          ],
        ),
      ),
      debugShowCheckedModeBanner: false,
    );
  }
}
```

- 提示底部超出溢出

![image-20220617230409553](/assets/image-20220617230409553.png)

- 清楚的显示了你溢出的情况，屏幕高 430 ，Column 被撑开 2070 , Container 高 2000 ， 底部溢出高度 1640.

![image-20220617230542902](/assets/image-20220617230542902.png)

---
title: Flutter-基础特性-有状态无状态组件
date: 2022-09-05 17:07:15
typora-root-url: ../
cover: /images/cover/flutter.png
top_img: false
tags: Flutter
comments: false
categories:
  - Flutter
  - flutter基础
---

# 有状态无状态组件

## 无状态 StatelessWidget

准备两张图片

```dart
const img1 =
    "https://ducafecat.tech/2021/12/09/blog/2021-jetbrains-fleet-vs-vscode/2021-12-09-10-30-00.png";
const img2 =
    "https://ducafecat.tech/2021/12/09/blog/2021-jetbrains-fleet-vs-vscode/2021-12-09-20-45-02.png";
```

编写图片显示组件 `BannerWidget`

```dart
class BannerWidget extends StatelessWidget {
  const BannerWidget({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Image.network(img1);
  }
}
```

`MyApp` 执行

```dart
class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: Column(
          children: const [
            Text('有无状态组件'),
            BannerWidget(),
          ],
        ),
      ),
      debugShowCheckedModeBanner: false,
    );
  }
}
```

运行

![image-20220618055552705](/assets/image-20220618055552705.png)

## 有状态 StatefulWidget

改写成有状态组件

```dart
class BannerWidget extends StatefulWidget {
  const BannerWidget({Key? key}) : super(key: key);

  @override
  State<BannerWidget> createState() => _BannerWidgetState();
}

class _BannerWidgetState extends State<BannerWidget> {
  String? imgUrl;

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        ElevatedButton(
          onPressed: () {
            setState(() {
              imgUrl = imgUrl == img1 ? img2 : img1;
            });
          },
          child: const Text("切换图片"),
        ),
        Image.network(imgUrl ?? img1),
      ],
    );
  }
}
```

运行

![image-20220618060738680](/assets/image-20220618060738680.png)

> 可以发现通过 `setState` 来管理和维护内部数据状态

## 有状态包裹无状态组件

编写图片显示组件

```dart
class ImageWidget extends StatelessWidget {
  const ImageWidget({Key? key, required this.imgUrl}) : super(key: key);

  final String imgUrl;

  @override
  Widget build(BuildContext context) {
    return Container(
        padding: const EdgeInsets.all(10),
        color: Colors.amber,
        child: Image.network(imgUrl));
  }
}
```

> 这里加了个 Container 显示边框

直接调用

```dart
      children: [
        ...
        ImageWidget(
          imgUrl: imgUrl ?? img1,
        ),
      ],
```

执行

![image-20220618062345553](/assets/image-20220618062345553.png)

> 可以发现很多组件都是无状态的，文本、图片、输入框、按钮、卡片、图标...

组件树

![image-20220618063344635](/assets/image-20220618063344635.png)

## 使用函数编写组件

编写函数组件

```dart
Widget imageWidget({required String imgUrl}) {
  return Container(
    padding: const EdgeInsets.all(10),
    color: Colors.amber,
    child: Image.network(imgUrl),
  );
}
```

调用

```dart
...
imageWidget(
  imgUrl: imgUrl ?? img1,
),
```

查看组件树

![image-20220618063630008](/assets/image-20220618063630008.png)

> 可以发现没有显示组件的名称 `imageWidget` 只是Container
>
> Flutter 这样做是出于性能考虑，所以如果你是可复用组件，需要用 class 包裹

## 参考

- [Flutter Stateless and Stateful Widget](https://medium.com/@paridhi.softinator/flutter-stateless-and-stateful-widget-4f1ef1fb7177)
- [Flutter: Stateful vs Stateless Widget](https://medium.com/flutter-community/flutter-stateful-vs-stateless-db325309deae)

## 错误处理

- 在 macos 中网络请求

macos 网络访问没有权限

修改文件 `macos/Runner/DebugProfile.entitlements` 和 `macos/Runner/Release.entitlements`

```
<key>com.apple.security.network.client</key>
<true/>
```

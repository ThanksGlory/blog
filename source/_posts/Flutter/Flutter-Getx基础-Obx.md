---
title: Flutter-Getx-Obx
date: 2022-09-08 23:48:27
typora-root-url: ../
cover: /images/cover/getx_cover.png
top_img: false
tags: Flutter
comments: false
categories:
  - Flutter
  - Getx
---

- GetX 是 Flutter 上的一个轻量且强大的解决方案：高性能的状态管理、智能的依赖注入和便捷的路由管理。
- GetX 有3个基本原则：
  - **性能：** GetX 专注于性能和最小资源消耗。GetX 打包后的apk占用大小和运行时的内存占用与其他状态管理插件不相上下。如果你感兴趣，这里有一个[性能测试](https://github.com/jonataslaw/benchmarks)。
  - **效率：** GetX 的语法非常简捷，并保持了极高的性能，能极大缩短你的开发时长。
  - **结构：** GetX 可以将界面、逻辑、依赖和路由完全解耦，用起来更清爽，逻辑更清晰，代码更容易维护。
- GetX 并不臃肿，却很轻量。如果你只使用状态管理，只有状态管理模块会被编译，其他没用到的东西都不会被编译到你的代码中。它拥有众多的功能，但这些功能都在独立的容器中，只有在使用后才会启动。
- Getx有一个庞大的生态系统，能够在Android、iOS、Web、Mac、Linux、Windows和你的服务器上用同样的代码运行。 **通过[Get Server](https://github.com/jonataslaw/get_server)** 可以在你的后端完全重用你在前端写的代码。

***lib/main.dart***

```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'ObxCount/index.dart';

void main() {
  runApp(const App());
}

class App extends StatelessWidget {
  const App({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    // 使用getx需要在此地方将 MaterialApp 替换成 GetMaterialApp
    return GetMaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: ObxCount(),
    );
  }
}
```

***lib/ObxCount/index.dart***

```dart
// ignore_for_file: must_be_immutable

import 'package:flutter/material.dart';
import 'package:get/get.dart';

class ObxCount extends StatelessWidget {
  // 定义1
  RxInt _count1 = RxInt(0);

  // 定义2
  var _count2 = 0.obs;

  // 定义3 可以用泛型也可以不填泛型
  final _count3 = Rx<double>(0);
  // final _count3 = Rx(0);

  ObxCount({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Gex --> Obx 三种定义'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [

            // 使用obs Rxint Rx 都可以写成 _count.value 其中 Rx只能写成 _count.value
            Obx(() => Text("定义1 数值：${_count1.value}")),
            Obx(() => Text("定义1 简写数值：$_count1")),
            Obx(() => Text("定义2 数值：${_count2.value}")),
            Obx(() => Text("定义2 简写数值：$_count2")),
            Obx(() => Text("定义3 数值：${_count3.value}")),
            ElevatedButton(
              onPressed: () {

                // 定义1
                _count1++;
                _count1.value++;

                // 定义2
                _count2++;
                _count2.value++;

                // 定义3
                _count3.value++;
              },
              child: const Text('计数器 点击加一'),
            ),
          ],
        ),
      ),
    );
  }
}
```

将组件用Obx进行包裹后可以实现无状态自动更新；

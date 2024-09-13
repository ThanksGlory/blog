---
title: Flutter-Getx-Getx生命周期
date: 2022-09-09 00:11:51
typora-root-url: ../
cover: /images/cover/getx_cover.png
top_img: false
tags: Flutter
categories:
  - Flutter
  - Getx
---

***lib/main.dart***

```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'GetxControllerLifeCycle/index.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    // 使用getx需要在此地方将 MaterialApp 替换成 GetMaterialApp
    return GetMaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const GetxControllerLifeCycle(),
    );
  }
}
```

***lib/GetxControllerLifeCycle/index.dart***

```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'GetxLifeCycleController.dart';

class GetxControllerLifeCycle extends StatelessWidget {
  const GetxControllerLifeCycle({super.key});

  @override
  Widget build(BuildContext context) {

   print("build...");

    return Scaffold(
      appBar: AppBar(
        title: const Text('Getx 生命周期控制器'),
      ),
      body: Center(
        child: Column(
          children: [
            GetBuilder<GetxLifeCycleController>(
              init: GetxLifeCycleController(),
              dispose: (state) {
                Get.find<GetxLifeCycleController>().cleanTask();
              },
              builder: (controller) {
                return Text('计数器数值：${controller.count}');
              },
            ),
            ElevatedButton(
              onPressed: () {
                Get.find<GetxLifeCycleController>().createTask();
              },
              child: const Text('点击加一'),
            )
          ],
        ),
      ),
    );
  }
}
```

***lib/GetxControllerLifeCycle/GetxLifeCycleController.dart***

```dart
// ignore_for_file: avoid_print
import 'package:get/get.dart';

class GetxLifeCycleController extends GetxController {
  var count = 0.obs;

  void createTask() {
    Future.delayed(const Duration(seconds: 3000), () {
      count++;
      update();
    });

    count++;
    update();
  }

  void cleanTask() {
    print('清除了任务');
  }

  @override
  void onInit() {
    super.onInit();
    print('初始化...');
  }

  @override
  void onReady() {
    super.onReady();
    print('加载完成...');
  }

  @override
  void onClose() {
    super.onClose();
    print('页面关闭...');
  }
}
```


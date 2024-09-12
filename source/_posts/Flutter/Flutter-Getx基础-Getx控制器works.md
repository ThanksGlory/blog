---
title: Flutter-Getx-Getx 控制器 works 事件监听
date: 2022-09-09 00:15:46
typora-root-url: ../
cover: /images/cover/getx_cover.png
top_img: false
tags: Flutter
comments: false
categories:
  - Flutter
  - Getx
---
***lib/main.dart***

```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'GetxControllerWorks/index.dart';

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
      home: GetxControllerWorks(),
    );
  }
}
```

***lib/GetxControllerWorks/index.dart***

```dart
// ignore_for_file: must_be_immutable

import 'package:flutter/material.dart';
import 'package:get/get.dart';

import 'getx_works_controller.dart';

class GetxControllerWorks extends StatelessWidget {
  GetxWorksController getxWorksController = Get.put(GetxWorksController());

  GetxControllerWorks({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('GetxControllerWorks'),
      ),
      body: Center(
        child: Column(
          children: [
            GetBuilder<GetxWorksController>(
              init: getxWorksController,
              builder: (controller) {
                return Column(
                  children: [
                    Text('计数器数值：${controller.count}'),
                    Text('输入框数值：${controller.inputValue}'),
                  ],
                );
              },
            ),
            ElevatedButton(
              onPressed: () {
                getxWorksController.addCount();
              },
              child: const Text('点击增加'),
            ),
            TextField(
              onChanged: (value) {
                getxWorksController.inputChange(value);
              },
            ),
          ],
        ),
      ),
    );
  }
}
```

***lib/GetxControllerWorks/getx_works_controller.dart***

```dart
// ignore_for_file: avoid_print

import 'package:get/get.dart';

class GetxWorksController extends GetxController {
  var count = 0.obs;
  var inputValue = ''.obs;

  @override
  void onInit() {
    super.onInit();

    // 监听count的值，当他发生改变的时候调用
    ever(count, (value) {
      print("ever ----> $count");
    });

    // 监听多个值，当他们发生改变的时候调用
    everAll([count], (value) => print("everAll ----> $count"));

    // 值改变时调用，只执行一次
    once(count, (value) => print("once ----> $count"));

    // 防抖 用户停止打字1秒钟后调用
    debounce(inputValue, (value) {
      value = "$value 拼接字符串";
      print("debounce callback ----> $value");
      print("debounce ----> $count");
    });

    // 忽略3秒中的所有改动
    interval(count, (value) => print("interval ----> $count"));
  }

  void addCount() {
    count++;
    update();
  }

  void inputChange(String value) {
    inputValue.value = value;
    update();
  }
}
```


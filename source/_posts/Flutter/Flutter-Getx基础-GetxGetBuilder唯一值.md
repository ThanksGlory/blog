---
title: Flutter-Getx-Getx GetBuilder 唯一值id
date: 2022-09-09 00:20:06
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
import 'GetControllerUniqueId/index.dart';

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
      home: const GetControllerUniqueId(),
    );
  }
}
```

***lib/GetControllerUniqueId/index.dart***

```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'GetUniqueIdController.dart';

class GetControllerUniqueId extends StatelessWidget {
  // GetUniqueIdController getUniqueIdController = Get.put(GetUniqueIdController());
  const GetControllerUniqueId({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Getx控制器唯一值id'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            GetBuilder<GetUniqueIdController>(
              init: GetUniqueIdController(),
              builder: (controller) {
                return Text(
                  '计数器值为：${controller.count}',
                  style: const TextStyle(
                    color: Colors.green,
                    fontSize: 40,
                  ),
                );
              },
            ),
            GetBuilder<GetUniqueIdController>(
              id: 'uni_id_count',
              init: GetUniqueIdController(),
              builder: (controller) {
                return Text(
                  '计数器值为：${controller.count}',
                  style: const TextStyle(
                    color: Colors.pink,
                    fontSize: 40,
                  ),
                );
              },
            ),
            const SizedBox(height: 20),
            Container(
              margin: const EdgeInsets.symmetric(horizontal: 20),
              width: double.infinity,
              child: ElevatedButton(
                onPressed: () {
                  Get.find<GetUniqueIdController>().increment();
                },
                child: const Text('增加'),
              ),
            )
          ],
        ),
      ),
    );
  }
}
```

***lib/GetControllerUniqueId/GetUniqueIdController.dart***

```dart
import 'package:get/get.dart';

class GetUniqueIdController extends GetxController{
  var count = 20;
  void increment(){
    count++;
    // 只会更新Getbuilder中id是uni_id_count的组件值
    update(['uni_id_count','']);
  }
}
```


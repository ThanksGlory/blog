---
title: Flutter-Getx-Getx控制器 controller
date: 2022-09-09 00:05:14
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
import 'GetControllerExp/index.dart';

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
      home: const GexControllerExp(),
    );
  }
}
```

***lib/GetControllerExp/index.dart***

```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'Expcontroller.dart';

// ignore: must_be_immutable
class GexControllerExp extends StatelessWidget {

  // Expcontroller expController = Expcontroller();
  // Expcontroller expController = Get.put(Expcontroller());
  const GexControllerExp({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('GexControllerExp'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            // 第一种
            // Obx(() => Text('我的名字是：${expController.teacher.name}')),
            // Obx(() => Text('我的年龄是：${expController.teacher.age}')),

            // 第二种
            // GetX<Expcontroller>(
            //   init: Expcontroller(),
            //   builder: (controller) {
            //     return Column(
            //       children: [
            //         // Text('我的名字是：${controller.teacher.name}'),
            //         // Text('我的年龄是：${controller.teacher.age}')
            //         Text('我的名字是：${controller.teacher.value.name}'),
            //         Text('我的年龄是：${controller.teacher.value.age}')
            //       ],
            //     );
            //   },
            // ),

            // 第三种方式
            GetBuilder<Expcontroller>(
              init: Expcontroller(),
              builder: (controller) {
                return Column(
                  children: [
                    Text('我的名字是：${controller.teacher.name.value}'),
                    Text('我的年龄是：${controller.teacher.age.value}'),
                    Text('test：${controller.text.value}'),
                  ],
                );
              },
            ),

            ElevatedButton(
              onPressed: () {
                // 第一种方式
                // expController.convertToUpperCase();
                // expController.ageAdd();
                // expController.testAdd();

                // 第二种
                Get.find<Expcontroller>().update();
                Get.find<Expcontroller>().convertToUpperCase();
                Get.find<Expcontroller>().ageAdd();
                Get.find<Expcontroller>().testAdd();
              },
              child: const Text('GexControllerExp 按钮'),
            )
          ],
        ),
      ),
    );
  }
}
```

***lib/GetControllerExp/Expcontroller.dart***

```dart
// ignore_for_file: file_names, non_constant_identifier_names

import 'package:get/get.dart';
import 'teacher.dart';


class Expcontroller extends GetxController {
  var text = 0.obs;
  // 第一种 对象属性是 obs 数据
  // var teacher = Teacher();
  // void ageAdd() {
  //   teacher.age.value++;
  // }

  // void convertToUpperCase() {
  //   teacher.name.value = teacher.name.value.toUpperCase();
  // }

  // 第二种 该对象是 obs 数据 对象数据必须 要嗲用 update
  // var teacher = Teacher(name: 'Augustus', age: 20).obs;

  // void ageAdd() {
  //   teacher.update((val) {
  //     val?.age = val.age + 1;
  //     // teacher.value.age++;
  //   });
  // }

  // void convertToUpperCase() {
  //   teacher.update((val) {
  //     val?.name = val.name.toUpperCase();
  //     // teacher.value.name = teacher.value.name.toUpperCase();
  //   });
  // }

  // 第三种 GetBuilder 必须要 在最后调用 update()
  var teacher = Teacher();
  void ageAdd() {
    teacher.age.value++;
  }

  void convertToUpperCase() {
    teacher.name.value = teacher.name.value.toUpperCase();
  }

  void testAdd(){
    text++;
  }
}
```

***lib/GetControllerExp/teacher.dart***

```dart
import 'package:get/get.dart';

class Teacher {
  // 第一种 写法属性 rx变量
  var name = "Christopher".obs;
  var age = 18.obs;

  // 第二种 构造函数创建
  // String name;
  // int age;
  // Teacher({required this.name, required this.age});
}
```


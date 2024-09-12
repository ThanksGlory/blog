---
title: Flutter-Getx-Obx自定义类
date: 2022-09-08 23:59:04
typora-root-url: ../
cover: /images/cover/getx_cover.png
top_img: false
tags: Flutter
comments: false
categories:
  - Flutter
  - Getx
---

**lib/main.dart**

```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'ObxCustomClass/index.dart';

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
      home: ObxCustomClass(),
    );
  }
}
```

**lib/ObxCustomClass/index.dart**

```dart
// ignore_for_file: must_be_immutable
import 'package:flutter/material.dart';
import 'package:flutter_gex/ObxCustomClass/teacher.dart';
import 'package:get/get.dart';

class ObxCustomClass extends StatelessWidget {

  // 第一种写法 纯属性
  // var teacher = Teacher();

  // 第二种写法 构造函数
  var teacher = Teacher(age: 30, name: 'Augustus').obs;

  ObxCustomClass({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Gex --> Obx 自定义类 '),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [

            // 第一种写法 纯属性
            // Obx(() => Text(
            //       '我的名字是：${teacher.name.value}',
            //       style: const TextStyle(
            //         color: Colors.redAccent,
            //         height: 40 / 20,
            //         fontSize: 20,
            //         fontWeight: FontWeight.bold,
            //       ),
            //     )),
            // Obx(
            //   () => Text(
            //     '我的年龄是：${teacher.age.value}',
            //     style: const TextStyle(
            //       color: Colors.green,
            //       height: 40 / 20,
            //       fontSize: 20,
            //       fontWeight: FontWeight.bold,
            //     ),
            //   ),
            // ),

            //  第二种写法 构造函数
            Obx(() => Text(
                  '我的名字是：${teacher.value.name}',
                  style: const TextStyle(
                    color: Colors.redAccent,
                    height: 40 / 20,
                    fontSize: 20,
                    fontWeight: FontWeight.bold,
                  ),
                )),
            Obx(
              () => Text(
                '我的年龄是：${teacher.value.age}',
                style: const TextStyle(
                  color: Colors.green,
                  height: 40 / 20,
                  fontSize: 20,
                  fontWeight: FontWeight.bold,
                ),
              ),
            ),
            ElevatedButton(
              onPressed: () {
                // 第一种写法 纯属性
                // teacher.name.value = teacher.name.value.toUpperCase();
                // teacher.age.value++;

                // 第二种写法 构造函数 如果是对象 则需要使用 update 方法
                teacher.update((val) {
                  // 1、可以使用 val Teacher 的实例对象
                  // val?.age = (val.age ?? 0) + 1;
                  // val?.name = (val.name ?? '').toUpperCase();
                  // 2、可以使用被 gex-->Rx 包装过的teacher对象
                  teacher.value.name = teacher.value.name?.toUpperCase();
                  teacher.value.age = (teacher.value.age ?? 0) + 1;
                });
              },
              child: const Text('Obx 自定义类'),
            ),
          ],
        ),
      ),
    );
  }
}
```

**lib/ObxCustomClass/teacher.dart**

```dart
class Teacher {
  // 第一种 写法属性
  // var name = "Christopher".obs;
  // var age = 18.obs;

  // 第二种 构造函数创建
  String? name;
  int? age;
  Teacher({this.name, this.age});
}
```

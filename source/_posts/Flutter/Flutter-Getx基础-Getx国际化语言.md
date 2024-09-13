---
title: Flutter-Getx-Getx国际化语言
date: 2022-09-09 18:53:50
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
import 'GetxInternationalization/Languages.dart';
import 'GetxInternationalization/index.dart';

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
      home: GetxInternationalization(),

      // 国际化配置
      translations: Languages(),
      locale: const Locale('zh','CN'), // 默认语言
      fallbackLocale: const Locale('zh','CN'), // 配置错误使用的语言
    );
  }
}
```

**translations**:  键值对映射类

**locale**：默认语言

**fallbackLocale**:  配置错误使用的语言



***lib/GetxInternationalization/Languages.dart***

```dart
import 'package:get/get.dart';

class Languages extends Translations{

  @override
  Map<String, Map<String, String>> get keys => {
    'zh_CN':{
      'hello':'你好，世界！'
    },
    'en_US':{
      'hello':'hello world!'
    },
  };
}
```

**注意:键一定是语言+下划线+国家**

***lib/GetxInternationalization/index.dart***

```dart
// ignore_for_file: must_be_immutable

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'LanguagesController.dart';

class GetxInternationalization extends StatelessWidget {
  LanguagesController languagesController = Get.put(LanguagesController());
  GetxInternationalization({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Getx 国际化语言'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              'hello'.tr,
              style: const TextStyle(
                color: Colors.pink,
                fontSize: 30,
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                languagesController.changeLanguage('zh', 'CN');
              },
              child: const Text('切换到中文'),
            ),
            ElevatedButton(
              onPressed: () {
                languagesController.changeLanguage('en', 'US');
              },
              child: const Text('切换到英文'),
            ),
          ],
        ),
      ),
    );
  }
}
```

也可以不需要controller

```dart
// ignore_for_file: must_be_immutable

import 'package:flutter/material.dart';
import 'package:get/get.dart';

class GetxInternationalization extends StatelessWidget {
  const GetxInternationalization({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Getx 国际化语言'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              'hello'.tr,
              style: const TextStyle(
                color: Colors.pink,
                fontSize: 30,
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                var locale = const Locale('zh', 'CN');
                Get.updateLocale(locale);
              },
              child: const Text('切换到中文'),
            ),
            ElevatedButton(
              onPressed: () {
                var locale = const Locale('en', 'US');
                Get.updateLocale(locale);
              },
              child: const Text('切换到英文'),
            ),
          ],
        ),
      ),
    );
  }
}

```

***lib/GetxInternationalization/LanguagesController.dart***

```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';

class LanguagesController extends GetxController {
  void changeLanguage(String languageCode, String countryCode) {
    Locale locale = Locale(languageCode, countryCode);
    Get.updateLocale(locale);
  }
}
```

示例：

![image-20220909190032648](/assets/image-20220909190032648.png)

![image-20220909190052243](/assets/image-20220909190052243.png)
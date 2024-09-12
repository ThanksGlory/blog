---
title: Flutter-基础特性-目录结构
date: 2022-09-04 21:14:06
typora-root-url: ../
cover: /images/cover/flutter.png
top_img: false
tags: Flutter
comments: false
categories:
  - Flutter
  - flutter基础
---

# 目录结构

## 创建 flutter 项目

- 使用 vscode 创建项目 【首选】

```shell
cmd + shift + p` 命令模式下 `flutter new project
```

![20220617160934](/assets/20220617160934.png)

选择 `Application` 类型

![20220617161715](/assets/20220617161715.png)

- 你也可以用 cli 方式 【可选】

```shell
$ flutter create flutter_quickstart_learn
```

- 如果你是用 `object-c` `java` 的朋友 【可选】

```shell
flutter create --ios-language=objc --android-language=java flutter_quickstart_learn
```

## 目录文件说明

```txt
.
├── android // 原生程序
│   ├── app
│   ├── gradle
│   ├── build.gradle
│   ├── flutter_quickstart_learn_android.iml
│   ├── gradle.properties
│   ├── gradlew
│   ├── gradlew.bat
│   ├── local.properties
│   └── settings.gradle
├── ios // 原生程序
│   ├── Flutter
│   ├── Runner
│   ├── Runner.xcodeproj
│   └── Runner.xcworkspace
├── lib // flutter dart 代码
│   └── main.dart
├── linux // linux 端
│   ├── flutter
│   ├── CMakeLists.txt
│   ├── main.cc
│   ├── my_application.cc
│   └── my_application.h
├── macos // macos 端
│   ├── Flutter
│   ├── Runner
│   ├── Runner.xcodeproj
│   └── Runner.xcworkspace
├── test // 测试目录
│   └── widget_test.dart
├── web // web 端
│   ├── icons
│   ├── favicon.png
│   ├── index.html
│   └── manifest.json
├── windows // windows 端
│   ├── flutter
│   ├── runner
│   └── CMakeLists.txt
├── README.md
├── analysis_options.yaml
├── flutter_quickstart_learn.iml
├── pubspec.lock // 包版本 lock
└── pubspec.yaml // flutter 配置文件，包管理 资源管理 配置管理
```

> 可以发现 flutter 是一个多端编译框架，各个平台对应的目录都有了

## 运行程序

能顺利运行，说明我们的环境没有问题，可以远航了。

![20220617163204](/assets/20220617163204.png)

## 参考

- https://docs.flutter.dev/reference/flutter-cli
- https://docs.flutter.dev/get-started/codelab

---
title: Dart语言学习-注释函数表达式-注释 comments
date: 2022-08-28 18:10:43
typora-root-url: ../
cover: /images/cover/dart.png
top_img: false
tags: Flutter
comments: false
categories:
  - Flutter
  - dart
---
# 注释

## 单行注释

```dart
// Symbol libraryName = new Symbol('dart.core');
```

## 多行注释

```dart
  /*
   * Symbol
   *
  Symbol libraryName = new Symbol('dart.core');
  MirrorSystem mirrorSystem = currentMirrorSystem();
  LibraryMirror libMirror = mirrorSystem.findLibrary(libraryName);
  libMirror.declarations.forEach((s, d) => print('$s - $d'));
  */
```

> 一般用在需要说明 类 函数 功能 输入 输出

## 文档注释

```dart
/// `main` 函数
///
/// 符号
/// 枚举
///
void main() {
  ...
}
```
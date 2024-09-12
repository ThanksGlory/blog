---
title: Dart语言学习-集合枚举-枚举 enum
date: 2022-08-28 16:36:14
typora-root-url: ../
cover: /images/cover/dart.png
top_img: false
tags: Flutter
comments: false
categories:
  - Flutter
  - dart
---

# 枚举

## 定义

适合用在常量定义，类型比较很方便。

```dart
// 定义颜色枚举值
enum ColorType {
  none,
  red,
  green,
  blue,
}

void main(List<String> args) {
  // 1 接收输入值 red 字符串
  var input = "red";

  // 2 将输入值转换为 ColorType 枚举类型
  var color = ColorType.none;
  if (input == "red") {
    color = ColorType.red;
  } else if (input == "green") {
    color = ColorType.green;
  } else if (input == "blue") {
    color = ColorType.blue;
  }

  // 3 switch 判断，打印输出
  switch (color) {
    case ColorType.red:
      print(ColorType.red);
      break;
    case ColorType.green:
      print(ColorType.green);
      break;
    case ColorType.blue:
      print(ColorType.blue);
      break;
    default:
      print(ColorType.none);
  }
}
```

## 使用场景

定义发帖类型 视频 图片 空

```dart
enum PostType { video, image, none }
```

业务中判断

```dart
  PostType get type {
    if (detail.type == POST_TYPE_IMAGE) {
      return PostType.image;
    } else if (detail.type == POST_TYPE_VIDEO) {
      return PostType.video;
    } else {
      return PostType.none;
    }
  }

  ...

  if (type == PostType.video && !_hasDetail) {
    playlist.insert(0, VideoMedia(detail));
  }

  ...

  if (type == PostType.video) {
      if (playlist.isEmpty) {
        playlist.add(VideoMedia(detail));
      } else {
        playlist.first.post = detail;
      }
    }
  } finally {
    _isLoading = false;
    if (type == PostType.video && !_hasDetail) getPostList();
  }
```

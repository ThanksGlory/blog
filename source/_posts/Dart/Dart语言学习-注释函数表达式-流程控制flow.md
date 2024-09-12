---
title: Dart语言学习-注释函数表达式-流程控制 flow
date: 2022-08-28 18:49:13
typora-root-url: ../
cover: /images/cover/dart.png
top_img: false
tags: Flutter
comments: false
categories:
  - Flutter
  - dart
---

# 流程控制

## if else

```dart
bool isPrint = true;
if (isPrint) {
  print('hello');
}
```

## for

```dart
for (var i = 0; i < 5; i++) {
  print(i);
}
```

## while

```dart
bool isDone = false;
while(!isDone) {
  print('is not done');
  isDone = true;
}
```

## do while

```dart
bool isRunning = true;
do {
  print('is running');
  isRunning = false;
} while (isRunning);
```

## switch case

```dart
String name = 'cat';
switch (name) {
  case 'cat':
    print('cat');
    break;
  default:
    print('not find');
}
```

## break

```dart
num i = 1;
while(true) {
  print('${i} - run');
  i++;
  if(i == 5) {
    break;
  }
}
```

## continue

```dart
for (var i = 0; i < 5; i++) {
  if (i < 3) {
    continue;
  }
  print(i);
}
```

## continue 指定位置

```dart
String command = "close";
switch (command) {
  case "open":
    print("open");
    break;
  case "close":
    print("close");
    continue doClear;
  case "close2":
    print("close2");
    continue doClear;

  doClear:
  case "doClose":
    print("doClose");
    break;

  default:
    print("other");
}
```

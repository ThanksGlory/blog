---
title: Dart语言学习-生成器 generate
date: 2022-09-03 22:03:35
typora-root-url: ../
cover: /images/cover/dart.png
top_img: false
tags: Flutter

categories:
  - Flutter
  - dart
---

# 生成器 generate

## 同步生成器

```dart
Iterable<int> naturalsTo(int n) sync* {
  print('start');
  int k = 0;
  while (k < n) {
    yield k++;
  }
  print('end');
}

main(List<String> args) {
  var it = naturalsTo(5).iterator;
  while (it.moveNext()) {
    print(it.current);
  }
}

start
0
1
2
3
4
end
```

> yield 会等待 `moveNext` 指令

## 异步生成器

```dart
import 'dart:async';

Stream<int> asynchronousNaturalsTo(int n) async* {
  print('start');
  int k = 0;
  while (k < n) {
    yield k++;
  }
  print('end');
}

main(List<String> args) {
  // 流监听
  // asynchronousNaturalsTo(5).listen((onData) {
  //   print(onData);
  // });

  // 流监听 StreamSubscription 对象
  StreamSubscription subscription = asynchronousNaturalsTo(5).listen(null);
  subscription.onData((value) {
    print(value);
    // subscription.pause();
  });
}

start
0
1
2
3
4
end
```

> 以流的方式一次性推送
>
> `StreamSubscription` 对象进行流监听控制

## 递归生成器

```dart
Iterable<int> naturalsDownFrom(int n) sync* {
  if (n > 0) {
    yield n;
    yield* naturalsDownFrom(n - 1);
  }
}

main(List<String> args) {
  var it = naturalsDownFrom(5).iterator;
  while (it.moveNext()) {
    print(it.current);
  }
}

5
4
3
2
1
```

> `yield*` 以指针的方式传递递归对象，而不是整个同步对象

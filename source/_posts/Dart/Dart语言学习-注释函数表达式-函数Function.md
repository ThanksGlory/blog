---
title: Dart语言学习-注释函数表达式-函数 Function
date: 2022-08-28 18:18:26
typora-root-url: ../
cover: /images/cover/dart.png
top_img: false
tags: Flutter
comments: false
categories:
  - Flutter
  - dart
---

# 函数

## 定义

```dart
int add(int x) {
  return x + 1;
}

调用
add(1);
```

## 可选参数

```dart
int add(int x, [int? y, int? z]) {
  if (y == null) {
    y = 1;
  }
  if (z == null) {
    z = 1;
  }
  return x + y + z;
}

调用
add(1, 2);
```

## 可选参数 默认值

```dart
int add(int x, [int y = 1, int z = 2]) {
  return x + y;
}

调用
int(1, 2);
```

## 命名参数 默认值

```dart
int add({int x = 1, int y = 1, int z = 1}) {
  return x + y + z;
}

调用
int(x: 1, y: 2);
```

## 函数内定义

```dart
void main(){
  int add(int x){
    return x + x;
  }
  print(add(1));
}
```

## Funcation 返回函数对象

```dart
Function makeAdd(int x) {
  return (int y) => x + y;
}

调用
var add = makeAdd(1);
print(add(5));
```

## 匿名函数

下面代码定义了只有一个参数 item 且没有参数类型的匿名方法。 List 中的每个元素都会调用这个函数，打印元素位置和值的字符串：

```dart
const list = ['apples', 'bananas', 'oranges'];
list.forEach((item) {
  print('${list.indexOf(item)}: $item');
});
```

箭头函数 如果只有一个表达式

```dart
list.forEach((item) => print('${list.indexOf(item)}: $item'));
```

## 作用域

下面是一个嵌套函数中变量在多个作用域中的示例：

```dart
bool topLevel = true;

void main() {
  var insideMain = true;

  void myFunction() {
    var insideFunction = true;

    void nestedFunction() {
      var insideNestedFunction = true;

      assert(topLevel);
      assert(insideMain);
      assert(insideFunction);
      assert(insideNestedFunction);
    }
  }
}
```

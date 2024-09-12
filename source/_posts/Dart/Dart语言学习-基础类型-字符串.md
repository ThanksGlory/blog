---
title: Dart语言学习-基础类型-字符串 String
date: 2022-08-24 23:15:12
typora-root-url: ../
cover: /images/cover/dart.png
top_img: false
tags: Flutter
comments: false
categories:
  - Flutter
  - dart
---

# 字符串

## 单引号或者双引号

- 赋值

```dart
String a = 'ducafecat';
String b = "ducafecat";
```

- 区别 转义分隔符

```dart
final myString = 'Bob\'s dog';            // Bob's dog
final myString = "a \"quoted\" word";     // a "quoted" word

final myString = "Bob's dog";             // Bob's dog
final myString = 'a "quoted" word';       // a "quoted" word

final value = '"quoted"';                 // "quoted"
final myString = "a $value word";         // a "quoted" word
```

## 字符串模板

```dart
var a = 123;
String b = 'ducafecat : ${a}';
print(b);
```

## 字符串连接

```dart
var a = 'hello' + ' ' + 'ducafecat';
var a = 'hello'' ''ducafecat';
var a = 'hello'   ' '     'ducafecat';

var a = 'hello'
' '
'ducafecat';

var a = '''
hello word
this is multi line
''';

var a = """
hello word
this is multi line
""";

print(a);
```

## 转义符号

```dart
var a = 'hello word \n this is multi line';
print(a);

hello word
 this is multi line
```

## 取消转义

```dart
var a = r'hello word \n this is multi line';
print(a);

hello word \n this is multi line
```

## 搜索

```dart
var a = 'web site ducafecat.tech';
print(a.contains('ducafecat'));
print(a.startsWith('web'));
print(a.endsWith('tech'));
print(a.indexOf('site'));

true
true
true
4
```

## 提取数据

```dart
var a = 'web site ducafecat.tech';
// substring截取位置为0~5的字符 不包含5
print(a.substring(0,5));
var b = a.split(' ');
print(b.length);
print(b[0]);

web s
3
web
```

## 大小写转换

```dart
var a = 'web site ducafecat.tech';
print(a.toLowerCase());
print(a.toUpperCase());

web site ducafecat.tech
WEB SITE DUCAFECAT.TECH
```

## 裁剪 判断空字符串

```dart
print('    hello word     '.trim());
print(''.isEmpty);

hello word
true
```

## 替换部分字符

```dart
print('hello word word!'.replaceAll('word', 'ducafecat'));

hello ducafecat ducafecat!
```

## 字符串创建

```dart
var sb = StringBuffer();
sb..write('hello word!')
..write('my')
..write(' ')
..writeAll(['web', 'site', 'https://ducafecat.tech']);
print(sb.toString());

hello word!my websitehttps://ducafecat.tech
```

---
title: Dart语言学习-集合枚举-集合 Map
date: 2022-08-28 16:07:01
typora-root-url: ../
cover: /images/cover/dart.png
top_img: false
tags: Flutter
comments: false
categories:
  - Flutter
  - dart
---

# Map

## 初始

key value 形式的集合，也叫键值对。

```dart
var a = {'name': 'ducafecat', 'web': 'www.ducafecat.tech'};

print(a);

{name: ducafecat, web: www.ducafecat.tech}
```

## 声明

### 松散

```dart
var a = Map();
a['name'] = 'ducafecat';
a['web'] = 'www.ducafecat.tech';
a[0] = 'abc';
a[0] = '1231231';

print(a);

{name: ducafecat, web: www.ducafecat.tech, 0: 1231231}
```

key 相同，覆盖数据

### 强类型

```dart
var a = Map<int, String>();
a[0] = 'java';
a[1] = 'python';

print(a);

{0: java, 1: python}
```

## 基本属性

| 名称       | 说明         |
| ---------- | ------------ |
| isEmpty    | 是否为空     |
| isNotEmpty | 是否不为空   |
| keys       | key 集合     |
| values     | values 集合  |
| length     | 个数         |
| entries    | 加工数据入口 |

```dart
var a = {'name': 'ducafecat', 'web': 'www.ducafecat.tech'};
print(a.isEmpty);
print(a.isNotEmpty);
print(a.keys);
print(a.values);
print(a.length);
print(a.entries);

false
true
(name, web)
(ducafecat, www.ducafecat.tech)
2
(MapEntry(name: ducafecat), MapEntry(web: www.ducafecat.tech))
```

## 常用方法

| 名称          | 说明          |
| ------------- | ------------- |
| addAll        | 添加          |
| addEntries    | 从入口添加    |
| containsKey   | 按 key 查询   |
| containsValue | 按 value 查询 |
| clear         | 清空          |
| remove        | 删除某个      |
| removeWhere   | 按条件删除    |
| update        | 更新某个      |
| updateAll     | 按条件更新    |

### addAll

新增 kv 数据

```dart
b.addAll({'first': 'java', 'second': 'python'});
```

### addEntries

接入另一个 map 集合

```dart
b.addEntries(a.entries);
```

### containsKey

检查 key 是否存在

```dart
print(a.containsKey('name'));
```

### containsValue

检查 value 是否存在

```dart
print(a.containsValue('www.ducafecat.tech'));
```

### clear

清空数据

```dart
b.clear();
```

### remove

删除数据

```dart
a.remove('name');
```

### removeWhere

按条件删除

```dart
a.removeWhere((key,val) => key == 'name');
```

### update

更新数据

```dart
a.update('name', (val) => 'abc');
```

### updateAll

更新批量

```dart
a.updateAll((key, val) => "---$val---");
```

## 操作符

| 名称 | 说明 |
| ---- | ---- |
| []   | 取值 |
| []=  | 赋值 |

```dart
print(a['name']);
a['name'] = 'abc';
```

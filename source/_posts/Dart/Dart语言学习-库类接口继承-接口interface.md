---
title: Dart语言学习-库类接口继承-接口 interface
date: 2022-09-03 15:50:44
typora-root-url: ../
cover: /images/cover/dart.png
top_img: false
tags: Flutter
comments: false
categories:
  - Flutter
  - dart
---

# 接口

- Dart 没有接口

https://dart.dev/samples#interfaces-and-abstract-classes

## 接口方式使用

- 定义人抽象类

```dart
abstract class IPerson {
  String name;
  int age;

  IPerson(this.name, this.age);

  String info() {
    return 'Name: $name, Age: $age';
  }
}
```

> 接口用途的抽象类 请用字母 `I` 开头 , 如 `IPhone`

- 老师

```dart
class Teacher implements IPerson {
  @override
  String name;

  @override
  int age;

  Teacher(this.name, this.age);

  @override
  String info() {
    return 'Teacher -> Name: $name, Age: $age';
  }
}
```

- 学生

```dart
class Student implements IPerson {
  @override
  int age;

  @override
  String name;

  Student(this.name, this.age);

  @override
  String info() {
    return 'Student -> Name: $name, Age: $age';
  }
}
```

- 打印信息

```dart
void makePersonInfo(IPerson user) => print(user.info());
```

- 实例化

```dart
void main(List<String> args) {
  var t = Teacher('ducafecat', 99);
  makePersonInfo(t);

  var s = Student('hans', 66);
  makePersonInfo(s);
}

Teacher -> Name: ducafecat, Age: 99
Student -> Name: hans, Age: 66
```

## 履行多接口

- 定义学校抽象类

```dart
abstract class ISchool {
  int grade;

  ISchool(this.grade);

  String schoolInfo() {
    return 'Grade: $grade';
  }
}
```

- 学生 多继承

```dart
class Student implements IPerson, ISchool {
  @override
  int age;

  @override
  String name;

  @override
  int grade;

  Student(this.name, this.age, this.grade);

  @override
  String info() {
    return 'Student -> Name: $name, Age: $age';
  }

  @override
  String schoolInfo() {
    return 'School -> Name: $name, Age: $age, Grade: $grade';
  }
}
```

- 打印信息

```dart
void makePersonInfo(IPerson user) => print(user.info());
void makeSchoolInfo(ISchool user) => print(user.schoolInfo());
```

- 实例化

```dart
void main(List<String> args) {
  var t = Teacher('ducafecat', 99);
  makePersonInfo(t);

  var s = Student('hans', 66, 5);
  makePersonInfo(s);
  makeSchoolInfo(s);
}

Teacher -> Name: ducafecat, Age: 99
Student -> Name: hans, Age: 66
School -> Name: hans, Age: 66, Grade: 5
```

## 从一个普通类履行接口

```dart
class Phone {
  void startup() {
    print('开机');
  }

  void shutdown() {
    print('关机');
  }
}

class AndroidPhone implements Phone {
  @override
  void startup() {
    print('AndroidPhone 开机');
  }

  @override
  void shutdown() {
    print('AndroidPhone 关机');
  }
}

void main() {
  var p = AndroidPhone();
  p.startup();
  p.shutdown();
}
```

> Dart 可以从一个普通的类履行接口

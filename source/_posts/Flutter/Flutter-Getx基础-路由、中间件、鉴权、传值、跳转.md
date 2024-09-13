---
title: Flutter-Getx-路由、中间件、鉴权、传值、跳转
date: 2022-09-10 19:31:30
typora-root-url: ../
cover: /images/cover/getx_cover.png
top_img: false
tags: Flutter

categories:
  - Flutter
  - Getx
---

# Getx

![2021-04-05-13-44-20](/assets/2021-04-05-13-44-20.png)

- GetPage 对象
- 路由层级控制
- 路由中间件、鉴权
- 404 处理
- 路由跳转、替换、清除
- 路由传值、返回值
- 路由转场动画

## 正文

### 初始 getx 项目

- pubspec.yaml

```yaml
dependencies:
  ...
  get: ^3.26.0
```

- lib/pages/home/index.dart

```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';

class HomeView extends StatelessWidget {
  const HomeView({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("首页"),
      ),
      body: ListView(
        children: [
          // 路由&导航
          Divider(),

        ],
      ),
    );
  }
}
```

- lib/common/routes/app_routes.dart

```dart
part of 'app_pages.dart';

abstract class AppRoutes {
  static const Home = '/home';
}
```

- lib/common/routes/app_pages.dart

```dart
import 'package:get/get.dart';

part 'app_routes.dart';

class AppPages {
  static const INITIAL = AppRoutes.Home;

  static final routes = [
    GetPage(
      name: AppRoutes.Home,
      page: () => HomeView(),
    ),
  ];
}
```

- lib/main.dart

```dart
import 'package:flutter/material.dart';
import 'package:flutter_gex/common/routes/app_pages.dart';
import 'package:get/get.dart';

void main() async {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    // 使用getx需要在此地方将 MaterialApp 替换成 GetMaterialApp
    return GetMaterialApp(
      title: 'Getx Demo',
      theme: ThemeData(
        primarySwatch: Colors.orange,
      ),

      debugShowCheckedModeBanner: false,

      // 初始路由
      initialRoute: AppPages.INITIAL,

      // 全部路由
      getPages: AppPages.routes,

      // 404未知页面
      unknownRoute: AppPages.unknownRoute,
    );
  }
}
```

### 编写 GetPage 定义

- lib/pages/list/index.dart

```dart
class ListView extends StatelessWidget {
  const ListView({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("列表页"),
      ),
    );
  }
}
```

- lib/pages/list_detail/index.dart

```dart
class DetailView extends StatelessWidget {
  const DetailView({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("详情页"),
      ),
      body: ListView(
        children: [
          ListTile(
            title: Text("导航-返回"),
            subtitle: Text('Get.back()'),
            onTap: () => Get.back(),
          ),
        ],
      ),
    );
  }
}
```

- lib/common/routes/app_routes.dart

```dart
abstract class AppRoutes {
  static const Home = '/home';
  static const List = '/list';
  static const Detail = '/detail';
```

- lib/common/routes/app_pages.dart

```dart
GetPage(
  name: AppRoutes.Home,
  page: () => HomeView(),
  children: [
    GetPage(
      name: AppRoutes.List,
      page: () => ListView(),
      children: [
        GetPage(
          name: AppRoutes.Detail,
          page: () => DetailView(),
        ),
      ],
    ),
  ],
),
```

### 导航操作 命名、视图对象

- lib/pages/home/index.dart

```dart
ListTile(
  title: Text("导航-命名路由 home > list"),
  subtitle: Text('Get.toNamed("/home/list")'),
  onTap: () => Get.toNamed("/home/list"),
),
ListTile(
  title: Text("导航-命名路由 home > list > detail"),
  subtitle: Text('Get.toNamed("/home/list/detail")'),
  onTap: () => Get.toNamed("/home/list/detail"),
),
ListTile(
  title: Text("导航-类对象"),
  subtitle: Text("Get.to(DetailView())"),
  onTap: () => Get.to(DetailView()),
),
```

### 导航-清除上一个

- lib/pages/home/index.dart

```dart
ListTile(
  title: Text("导航-清除上一个"),
  subtitle: Text("Get.off(DetailView())"),
  onTap: () => Get.off(DetailView()),
),
```

### 导航-清除所有

- lib/pages/home/index.dart

```dart
ListTile(
  title: Text("导航-清除所有"),
  subtitle: Text("Get.offAll(DetailView())"),
  onTap: () => Get.offAll(DetailView()),
),
```

### 导航-arguments 传值+返回值

- lib/pages/home/index.dart

```dart
 // 第一种方式传值 arguments 对象传值
ListTile(
  title: Text("导航-arguments传值+返回值"),
  subtitle: Text(
      'Get.toNamed("/home/list/detail", arguments: {"id": 999})'),
  onTap: () async {
    var result = await Get.toNamed("/home/list/detail",
        arguments: {"id": 999});
    Get.snackbar("返回值", "success -> " + result["success"].toString());
  },
),
```

- lib/pages/list_detail/index.dart

```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';

class ListDetailPage extends StatelessWidget {
  const ListDetailPage({Key? key}) : super(key: key);

  Widget _buildBackListTileRow(Map? val) {
    if (val == null || val.isEmpty) return Container();
    return ListTile(
      title: Text("传值 id = ${val["id"]}"),
      subtitle: const Text('Get.back(result: {"success": true}'),
      onTap: () => Get.back(result: {"success": true}),
    );
  }

  @override
  Widget build(BuildContext context) {

    // 第一种方式arguments对象传值
    // Get.arguments对象可能会为null对象 所有要有空安全检查 以Map?去接收值
    final Map? details = Get.arguments;

    // 第二种方式parameters querystring方式传值
    // Get.parameters是一定会存在的一个对象 只不过可能会为空对象 可以直接用Map去接收值
    final Map parameters = Get.parameters;

    return Scaffold(
      appBar: AppBar(
        title: const Text("详情页"),
      ),
      body: ListView(
        children: [
          ListTile(
            title: const Text("导航-返回"),
            subtitle: const Text('Get.back()'),
            // Get.back可以传值也可以不传值 Get.back() or Get.back(result: {"success": true})
            onTap: () => Get.back(),
          ),
          _buildBackListTileRow(details),
          _buildBackListTileRow(parameters)
        ],
      ),
    );
  }
}

```

### 导航-parameters 传值+返回值

- lib/pages/home/index.dart

```dart
 // 第二种方式传值 /home/list/list_detail?id=666 querystring 方式传值
ListTile(
  title: Text("导航-parameters传值+返回值"),
  subtitle: Text('Get.toNamed("/home/list/detail?id=666")'),
  onTap: () async {
    var result = await Get.toNamed("/home/list/detail?id=666");
    Get.snackbar("返回值", "success -> " + result["success"].toString());
  },
),
```

- lib/pages/list_detail/index.dart

```dart
@override
Widget build(BuildContext context) {
  final parameters = Get.parameters;
```

### 导航-参数传值+返回值

- lib/common/routes/app_routes.dart

```dart
static const Detail_ID = '/detail/:id';
```

- lib/common/routes/app_pages.dart

```dart
...
GetPage(
  name: AppRoutes.Detail_ID,
  page: () => DetailView(),
),
```

- lib/pages/home/index.dart

```dart
// 第三种方式传值 /home/list/list_detail/777 querystring 方式传值
ListTile(
  title: Text("导航-参数传值+返回值"),
  subtitle: Text('Get.toNamed("/home/list/detail/777")'),
  onTap: () async {
    var result = await Get.toNamed("/home/list/detail/777");
    Get.snackbar("返回值", "success -> " + result["success"].toString());
  },
),
```

### 导航-not found

- lib/pages/notfound/index.dart

```dart
class NotfoundView extends StatelessWidget {
  const NotfoundView({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("路由没有找到"),
      ),
      body: ListTile(
        title: Text("返回首页"),
        subtitle: Text('Get.offAllNamed(AppRoutes.Home)'),
        onTap: () => Get.offAllNamed(AppRoutes.Home),
      ),
    );
  }
}
```

- lib/common/routes/app_routes.dart

```dart
static const NotFound = '/notfound';
```

- lib/common/routes/app_pages.dart

```dart
static final unknownRoute = GetPage(
  name: AppRoutes.NotFound,
  page: () => NotfoundView(),
);
```

- lib/main.dart

```dart
@override
Widget build(BuildContext context) {
  return GetMaterialApp(
    ...
    unknownRoute: AppPages.unknownRoute,
  );
}
```

### 导航-中间件-认证 Auth

- lib/pages/login/index.dart

```dart
class LoginView extends StatelessWidget {
  const LoginView({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("登录"),
      ),
      body: ListTile(
        title: Text("返回首页"),
        subtitle: Text('Get.offAllNamed(AppRoutes.Home)'),
        onTap: () => Get.offAllNamed(AppRoutes.Home),
      ),
    );
  }
}
```

- lib/pages/my/index.dart

```dart
class MyView extends StatelessWidget {
  const MyView({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("我的"),
      ),
      body: ListTile(
        title: Text("返回首页"),
        subtitle: Text('Get.offAllNamed(AppRoutes.Home)'),
        onTap: () => Get.offAllNamed(AppRoutes.Home),
      ),
    );
  }
}
```

- lib/common/routes/app_routes.dart

```dart
static const Login = '/login';
static const My = '/my';
```

- lib/common/middleware/router_auth.dart

```dart
class RouteAuthMiddleware extends GetMiddleware {
  // priority 代表如果存在多个中间件哪个的优先级高
  @override
  int priority = 0;

  RouteAuthMiddleware({required this.priority});

  @override
  RouteSettings? redirect(String route) {
    Future.delayed(Duration(seconds: 1), () => Get.snackbar("提示", "请先登录APP"));
    return RouteSettings(name: AppRoutes.Login);
  }
}
```

- lib/common/routes/app_pages.dart

```dart
// 白名单
GetPage(
  name: AppRoutes.Login,
  page: () => LoginView(),
),

GetPage(
  name: AppRoutes.My,
  page: () => MyView(),
  middlewares: [
    RouteAuthMiddleware(priority: 1),
  ],
),
```

- lib/pages/home/index.dart

```dart
ListTile(
  title: Text("导航-中间件-认证Auth"),
  subtitle: Text('Get.toNamed(AppRoutes.My)'),
  onTap: () => Get.toNamed(AppRoutes.My),
),
```

### Transition 转场动画

- lib/common/routes/app_pages.dart

```dart
GetPage(
  name: AppRoutes.Detail_ID,
  page: () => DetailView(),
  transition: Transition.downToUp,
),
```

---
title: React项目结构完整版
date: 2022-08-26 21:00:52
typora-root-url: ../
cover: /images/cover/react_directory.png
top_img: false
tags: React
categories:
  - React
---

```txt
root
├─node_modules(项目依赖包文件目录)
├─public(资源目录)
│	  ├─configs(存放项目打包后部署需要修改的全局变量 例如：WebSocket地址)
│     ├─assets(存放项目静态文件 例如：图片、3d模型)
│     │  	 ├─images(图片)
│     │      ├─models(模型3d模型)
│     │      ...
│     ├─docs(文档 系统版本)
│     │    ├-images(修改文档图片目录)
│     │    ├─doc.html(版本介绍、修改内容文档、修改版本号记录)
│     │    ├─软件使用说明.docx
│     │    ...
│     ├─libs(存放需要CDN引入的第三方插件库)
│     │	  ...
│     ├─theme(存放系统皮肤less文件，系统存在换肤时存在否则可以删除)
│     │	    └─color.less(系统皮肤颜色文件)
│     ├─data(存放系统临时数据文件 例如:data.json)
│     ├─favicon.ico(系统页签小图标)
│     ├─index.html(页面模板页面)
│     ...
├─src(源码目录)
│   ├─pages(系统页面目录、按照系统导航栏进行页面目录搭建)
│   │    ├-home(首页目录)
│   │    │   ├-index.ts
│	│    │   ├-index.less
│   │    ├-login(登录目录)
│   │    │   ├-index.ts
│	│    │   ├-index.less
│   │    ...
│   ├-@types(存放系统类型文件)
│   │    ├-global.d.ts
│   │    ├-navMenu.ts
│   │    ...
│   ├-components(存放系统业务组件，组件内部可能含有http请求)
│   ├-constants(存放系统常量)
│   ├-hooks(存放自定义钩子)
│   ├-http(存放http请求方法，axios封装的请求方法，拦截器)
│   ├-routers(存放系统路由文件)
│   │      ├-allComponents.ts(所有页面组件集合)
│   │      ├-MainRouter.tsx(主路由)
│   │      ├-routerConfig.ts(路由配置文件)
│   │      └─SubRouter.tsx(子路由)
│   ├-assets(存放开发静态文件)
│   ├-store(系统通信数据仓库)
│   ├-styles(通用样式目录)
│   ├-theme(换肤目录，系统存在换肤时存在否则可以删除)
│   │    ├-dark.ts
│   │    └─light.ts
│   ├-store(存放系统通信数据仓库)
│   ├-utils(工具类目录)
│   ├-widgets(纯UI组件目录)
│   ├-App.test.tsx(测试文件)
│   ├-App.tsx(应用入口)
│   ├-index.tsx(系统主入口)
│   ...
├-theme(换肤文件生成目录，系统存在换肤时存在否则可以删除)
├-.gitignore(git上传忽略文件)
├-craco.cofig.js(打包，代理配置文件)
├-package.json(依赖包文件)
├-README.md
├-tsconfig.json(ts配置文件)
├-tsconfig.path.json(ts路径配置文件)
└─yarn.lock(yarn安装依赖版本锁文件)
```


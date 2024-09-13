---
title: React Typescript 项目初始化
date: 2022-03-04 16:03:50
typora-root-url: ../
cover: /images/cover/react.jpg
top_img: false
tags: React
categories:
  - TypeScript
  - React
---

<center><font size="5">从零开始搭建一个React TypeScript项目</font></center>

最近开始使用 React 和 TypeScript 开发项目了，顺便整理一下 react-ts 项目的创建过程。

node/npm 这些基本的东西就不再赘述了，没有的自行安装一下。

### 全局安装 create-react-app脚手架

如果很久之前安装过，建议卸载重新安装 npm uninstall -g create-react-app

```shell
npm install -g create-react-app
```

### 创建项目

```sh
npx create-react-app my-app --template typescript
```

### webpack

执行npm run eject 放开配置项，个人不是很建议使用craco.config.js；

```sh
npm run eject
```

#### 配置less

1、安装 less less-loader

```sh
yarn add less less-loader -D
# OR
npm install less less-loader -D
```

2、修改webpack.config.js

```js
// style files regexes
const cssRegex = /\.(css|less)$/; // 加上less选项
const cssModuleRegex = /\.module\.(css|less)$/; // 加上less选项

// ...

module.exports = function (webpackEnv) {
    // ...
    resolve: {
        extensions: paths.moduleFileExtensions
            .map(ext => `.${ext}`)
            .filter(ext => useTypeScript || !ext.includes('ts')),
    },
    module: {
        rules: [
            {
              test: cssRegex,
              exclude: cssModuleRegex,
              use: getStyleLoaders({
                importLoaders: 1,
                sourceMap: isEnvProduction
                  ? shouldUseSourceMap
                  : isEnvDevelopment,
              },
              'less-loader'),  // 加上 less-loader
              // Don't consider CSS imports dead code even if the
              // containing package claims to have no side effects.
              // Remove this when webpack adds a warning or an error for this.
              // See https://github.com/webpack/webpack/issues/6571
              sideEffects: true,
            },
            // Adds support for CSS Modules (https://github.com/css-modules/css-modules)
            // using the extension .module.css
            {
              test: cssModuleRegex,
              use: getStyleLoaders({
                importLoaders: 1,
                sourceMap: isEnvProduction
                  ? shouldUseSourceMap
                  : isEnvDevelopment,
                modules: {
                  getLocalIdent: getCSSModuleLocalIdent,
                },
              },
              'less-loader'),  //加上 less-loader
            },
        ]
    }
}
```

#### webpack 进度条

1、安装 webpackbar

```sh
yarn add webpackbar -D
# OR
npm install webpackbar -D
```

2、修改webpack.config.js

```js
const WebpackBar = require("webpackbar");

// ...

module.exports = function (webpackEnv) {

    // ...
   plugins:[
       new WebpackBar({ profile: true }),
   ]
}
```

#### 兼容IE 11

1、安装 @babel/preset-env  @babel/polyfill

```sh
yarn add @babel/preset-env @babel/polyfill core-js -S
```

2、修改package.json

修改浏览器配置列表browserslist，将`"ie > 8"`加到下面两个数组中；

```json
"browserslist": {
    "production": [
        ">0.2%",
        "not dead",
        "not op_mini all",
        "ie > 8"
    ],
    "development": [
        "last 1 chrome version",
        "last 1 firefox version",
        "last 1 safari version",
        "ie > 8"
    ]
}
```

3、添加兼容代码

src/index.js 或者 src/index.tsx

```tsx
import '@babel/polyfill';
import "core-js/es/map"

import React from 'react';
import ReactDOM from 'react-dom';
import reportWebVitals from './reportWebVitals';
import App from './App';

ReactDOM.render(<App />, document.getElementById('root'));
reportWebVitals();
```

4、修改webpack.config.js

```js
module.exports = {
    //webpack配置
    webpack: {
        entry: ["@babel/polyfill", paths.appIndexJs],
        // 配置别名
        alias: {//设置别名是为了让后续引用的地方减少路径的复杂度
            "@": path.resolve("src"),
            "@utils": path.resolve("src/utils"),
        }
    },
}
```

5、babel配置

修改package.json

```json
"babel": {
    presets: [
        "react-app",
        "@babel/preset-env"
    ]
}
```

### 添加装饰器(decorators)

1、安装插件 @babel/plugin-proposal-decorators

```sh
yarn add @babel/plugin-proposal-decorators -D
```

2、修改package.json

```json
"babel": {
   plugins: [
     ["@babel/plugin-proposal-decorators", { legacy: true }],
   ]
}
```

### antd按需导入

style : true  设置为true即是less，当设置为true的时候可能会报错，如下图所示：

![antd_less](/assets/antd_less.png)

建议使用css style : "css";

```sh
yarn add babel-plugin-import -D
# OR
npm install babel-plugin-import -D
```

修改package.json

```json
"babel": {
   plugins: [
      [
          "import",
          {
              "libraryName": "antd",
              "libraryDirectory": "es",
              "style": "css"
          }
      ]
   ]
}
```

### antd主题色修改

1、根目录创建 theme 文件夹，包含一下文件，如下图所示：

![theme20220305](/assets/theme20220305.png)

2、安装 antd-theme-generator

```sh
yarn add antd-theme-generator -D
# OR
npm install antd-theme-generator -D
```

3、index.js

```js
const path = require('path');
const { themeVariables } = require('./themeVariables');
const { generateTheme } = require('antd-theme-generator');

const options = {
    stylesDir: path.join(__dirname, '../src/styles'),
    antDir: path.join(__dirname, '../node_modules/antd'),
    varFile: path.join(__dirname, '../src/styles/variables.less'),
    mainLessFile: path.join(__dirname, '../src/styles/index.less'),
    //需要动态切换的主题变量
    themeVariables,
    indexFileName: 'index.html',
    outputFilePath: path.join(__dirname, '../public/theme/color.less') //页面引入的主题变量文件
};

generateTheme(options).then(less => {
    console.log('Theme generated successfully');
}).catch(error => {
    console.log('Error', error);
});
```

4、themeVariables.js

```js
exports.themeVariables = [
    '@primary-color',
    '@secondary-color',
    '@text-select',
    '@text-color-secondary',
    '@heading-color',
    '@layout-body-background'
];
```

5、在src下面创建 styles

通用样式放index.less 变量样式 variables.less

variables.less

```less
@import '~antd/lib/style/themes/default.less'; //引入antd的变量文件，实现变量的覆盖

// 标题颜色
@title: #1f1f1f;

// 正文颜色
@text: #3f3f3f;

// 文字框选颜色
@text-select: #a6d997;
```

6、public文件夹

1).在public下面创建libs，再在libs创建less文件夹将less.js放到less文件夹中，less版本必须为低版本，这里用的v2.7.2  public/libs/less/less.js 版本v2.7.2

2).修改public/index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#000000" />
    <meta
      name="description"
      content="Web site created using create-react-app"
    />
    <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
    <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
    <title>来也文旅</title>
  </head>

  <body>
    <!--这里link放在哪，style生成在哪里，注意样式被覆盖-->
    <link rel="stylesheet/less" type="text/css" href="/theme/color.less" />
    <script>
      // production  development
      window.less = { async: false, env: 'production' };
    </script>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
    <script src="/libs/less/less.js"></script>
  </body>
</html>
```

3).在public下面创建 theme文件夹 生成的color.less 会放到该文件夹下面；

4).修改启动项package.json

```json
{
  "scripts": {
    "start": "node theme && node scripts/start.js",
    "build": "node theme && node scripts/build.js",
    "test": "node scripts/test.js",
  },
}
```

5).在页面中使用

```js
window.less.modifyVars({
    "@text-select": "#00ffe7"
}).catch(err => {
    console.log("改变主题失败！！！")
});
```

6).less在修改的时候会将body的样式设置为display:none;导致获取宽高可能出问题，所以我们可以在自定义样式中设置以下样式：

```less
body{
  display:none !importent;
}
```






















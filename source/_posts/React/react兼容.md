---
title: react兼容IE11
date: 2022-02-15 17:17:36
tags: React
cover: /images/cover/javascript_context.jpg
top_img: false
typora-root-url: ../
categories:
  - React
---

<center>react ie9版本以上兼容问题</center>

#### 插件安装

```sh
yarn add @babel/preset-env @babel/polyfill -S
```

#### 修改package.json

修改浏览器配置列表browserslist

将` "ie > 8" `加到下面两个数组中

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
},
```

#### 添加兼容代码

src/index.js

必须在index.js中的首行添加兼容导入代码

```js
import '@babel/polyfill';
import "core-js/es/map"

import React from 'react';
import ReactDOM from 'react-dom';
import reportWebVitals from './reportWebVitals';
import App from './App';

ReactDOM.render(<App />, document.getElementById('root'));
reportWebVitals();
```

#### webpack配置

craco.config.js

```js
module.exports = {
    //webpack配置
    webpack: {
        entry: ["@babel/polyfill", "./src/index.js"],
        // 配置别名
        alias: {//设置别名是为了让后续引用的地方减少路径的复杂度
            "@": path.resolve("src"),
            "@utils": path.resolve("src/utils"),
        },
        // webpack插件
        plugins: [
            new WebpackBar({ profile: true }),
            new CircularDependencyPlugin({
                exclude: /node_modules/,
                include: /src/,
                failOnError: true,
                allowAsyncCycles: false,
                cwd: process.cwd()
            }),
        ]
    },
}
```

#### babel配置

craco.config.js

```js
module.exports = {
    presets: [
        "@babel/preset-env"
    ],
    plugins: [
        //支持装饰器
        ["@babel/plugin-proposal-decorators", { legacy: true }],
        // antd按需导入
        [
            "import",
            {
                "libraryName": "antd",
                "libraryDirectory": "es",
                "style": true //设置为true即是less 这里用的是css
            }
        ]
    ]
}
```


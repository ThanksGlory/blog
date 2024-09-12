---
title: craco.config.js
date: 2022-01-28 14:50:39
typora-root-url: ../
cover: /images/cover/crco.png
top_img: false
tags: React
comments: false
categories:
  - React
---

### 配置步骤

1、首先，使用 `create-react-app` 创建一个项目，这里我们命名为 `my-app`;

```sh
npx create-react-app my-app
```

2、进入项目目录，安装基本依赖

```sh
yarn add antd @craco/craco @babel/plugin-proposal-decorators babel-plugin-import -D
```

**注意：npm 安装回出现问题，这里建议用yarn进行安装依赖**

3、修改 `package.json` 中的 `scripts`

原来的`package.json`

```json
{
  "scripts":{
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test"
  }
}
```

修改后的`package.json`

```json
{
  "scripts":{
    "start": "craco start",
    "build": "craco build",
    "test": "craco test"
  }
}
```

4、项目根目录创建 `craco.config.js` 文件

[craco.config.js官方链接](https://github.com/gsoft-inc/craco/tree/master/packages/craco)

```js
const { when, whenDev, whenProd, whenTest, ESLINT_MODES, POSTCSS_MODES } = require("@craco/craco");

module.exports = {
  reactScriptsVersion: "react-scripts" /* (default value) */,
  style: {
    modules: {
      localIdentName: ""
    },
    css: {
      loaderOptions: { /* Any css-loader configuration options: https://github.com/webpack-contrib/css-loader. */ },
      loaderOptions: (cssLoaderOptions, { env, paths }) => { return cssLoaderOptions; }
    },
    sass: {
      loaderOptions: { /* Any sass-loader configuration options: https://github.com/webpack-contrib/sass-loader. */ },
      loaderOptions: (sassLoaderOptions, { env, paths }) => { return sassLoaderOptions; }
    },
    postcss: {
      mode: "extends" /* (default value) */ || "file",
      plugins: [require('plugin-to-append')], // Additional plugins given in an array are appended to existing config.
      plugins: (plugins) => [require('plugin-to-prepend')].concat(plugins), // Or you may use the function variant.
      env: {
        autoprefixer: { /* Any autoprefixer options: https://github.com/postcss/autoprefixer#options */ },
        stage: 3, /* Any valid stages: https://cssdb.org/#staging-process. */
        features: { /* Any CSS features: https://preset-env.cssdb.org/features. */ }
      },
      loaderOptions: { /* Any postcss-loader configuration options: https://github.com/postcss/postcss-loader. */ },
      loaderOptions: (postcssLoaderOptions, { env, paths }) => { return postcssLoaderOptions; }
    }
  },
  eslint: {
    enable: true /* (default value) */,
    mode: "extends" /* (default value) */ || "file",
    configure: { /* Any eslint configuration options: https://eslint.org/docs/user-guide/configuring */ },
    configure: (eslintConfig, { env, paths }) => { return eslintConfig; },
    pluginOptions: { /* Any eslint plugin configuration options: https://github.com/webpack-contrib/eslint-webpack-plugin#options. */ },
    pluginOptions: (eslintOptions, { env, paths }) => { return eslintOptions; }
  },
  babel: {
    presets: [],
    plugins: [],
    loaderOptions: { /* Any babel-loader configuration options: https://github.com/babel/babel-loader. */ },
    loaderOptions: (babelLoaderOptions, { env, paths }) => { return babelLoaderOptions; }
  },
  typescript: {
    enableTypeChecking: true /* (default value)  */
  },
  webpack: {
    alias: {},
    plugins: {
      add: [], /* An array of plugins */
      add: [
        plugin1,
        [plugin2, "append"],
        [plugin3, "prepend"], /* Specify if plugin should be appended or prepended */
      ], /* An array of plugins */
      remove: [],  /* An array of plugin constructor's names (i.e. "StyleLintPlugin", "ESLintWebpackPlugin" ) */
    },
    configure: { /* Any webpack configuration options: https://webpack.js.org/configuration */ },
    configure: (webpackConfig, { env, paths }) => { return webpackConfig; }
  },
  jest: {
    babel: {
      addPresets: true, /* (default value) */
      addPlugins: true  /* (default value) */
    },
    configure: { /* Any Jest configuration options: https://jestjs.io/docs/en/configuration */ },
    configure: (jestConfig, { env, paths, resolve, rootDir }) => { return jestConfig; }
  },
  devServer: { /* Any devServer configuration options: https://webpack.js.org/configuration/dev-server/#devserver */ },
  devServer: (devServerConfig, { env, paths, proxy, allowedHost }) => { return devServerConfig; },
  plugins: [
    {
      plugin: {
        overrideCracoConfig: ({ cracoConfig, pluginOptions, context: { env, paths } }) => { return cracoConfig; },
        overrideWebpackConfig: ({ webpackConfig, cracoConfig, pluginOptions, context: { env, paths } }) => { return webpackConfig; },
        overrideDevServerConfig: ({ devServerConfig, cracoConfig, pluginOptions, context: { env, paths, proxy, allowedHost } }) => { return devServerConfig; },
        overrideJestConfig: ({ jestConfig, cracoConfig, pluginOptions, context: { env, paths, resolve, rootDir } }) => { return jestConfig },
      },
      options: {}
    }
  ]
};
```

### 配置webpack

```js
module.exports = {
   webpack: {//webpack配置
    // 配置别名
    alias: {//设置别名是为了让后续引用的地方减少路径的复杂度
      "@": path.resolve("src"),
      "@utils": path.resolve("src/utils"),
    }
  },
}
```

#### 安装webpack插件

安装插件 WebpackBar 打包进度条

```sh
yarn add webpackbar -D
```

安装插件 circular-dependency-plugin依赖插件

```sh
yarn add circular-dependency-plugin -D
```

#### 配置插件

```js
const WebpackBar = require("webpackbar");
const CircularDependencyPlugin = require("circular-dependency-plugin");

module.exports = {
  // webpack配置
  webpack: {
    // 配置别名，设置别名是为了让后续引用的地方减少路径的复杂度
    alias: {
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
      })
    ]
  },
}
```



### 配置babel

```js
module.exports = {
  babel: {
    presets: ["@babel/preset-env"],
    plugins: [
      //支持装饰器
      ["@babel/plugin-proposal-decorators", { legacy: true }],
      // antd按需导入
      [
        "import",
        {
          "libraryName": "antd",
          "libraryDirectory": "es",
          "style": 'css' //设置为true即是less 这里用的是css
        }
      ]
    ]
  },
}
```

### 配置module(loader)

```js
module.exports = {
  module: {
    rules: [ //规则，在写style.module.scss的时候发现引入后缀为.scss会报错，在这里配置一下即可
      {
        test: /.scss$/,
        loaders: ['style-loader', 'css-loader', 'sass-loader'],
      }
    ]
  },
}
```

### 配置devServer

```js
module.exports = {
  devServer: {
    port: 8888,
    // 配置代理解决跨域
    proxy: {
      "/api": {
        target: 'http://XXXXXXXX:8888',
        changeOrigin: true,
        pathRewrite: {
          "^/api": ""
        }
      }
    }
  },
}
```

### craco插件

#### less支持

```sh
yarn add less less-loader craco-less -D
```

#### 配置less

craco.config.js

```js
const CracoLessPlugin = require("craco-less");
module.exports = {
  plugins: [
    {
      plugin: CracoLessPlugin,
      options: {
        lessLoaderOptions: {
          lessOptions: {
            modifyVars: {},
            javascriptEnabled: true,
          },
        },
      },
    },
  ],
}
```

### 最终craco.config.js

```js
const path = require('path');
const WebpackBar = require("webpackbar");
const CircularDependencyPlugin = require("circular-dependency-plugin");
const CracoLessPlugin = require("craco-less");

module.exports = {
  // webpack配置
  webpack: {
    // 配置别名，设置别名是为了让后续引用的地方减少路径的复杂度
    alias: {
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
      })
    ]
  },
  babel: {
    presets: ["@babel/preset-env"],
    plugins: [
      //支持装饰器
      ["@babel/plugin-proposal-decorators", { legacy: true }],
      // antd按需导入
      [
        "import",
        {
          "libraryName": "antd",
          "libraryDirectory": "es",
          "style": 'css' //设置为true即是less 这里用的是css
        }
      ]
    ]
  },
  module: {
    rules: [ //规则，在写style.module.scss的时候发现引入后缀为.scss会报错，在这里配置一下即可
      {
        test: /.scss$/,
        loaders: ['style-loader', 'css-loader', 'sass-loader'],
      }
    ]
  },

  devServer: {
    port: 8888,
    // 配置代理解决跨域
    proxy: {
      "/api": {
        target: 'http://XXXXXXXX:8888',
        changeOrigin: true,
        pathRewrite: {
          "^/api": ""
        }
      }
    }
  },
  plugins: [
    {
      // 配置less支持
      plugin: CracoLessPlugin,
      options: {
        lessLoaderOptions: {
          lessOptions: {
            modifyVars: {},
            javascriptEnabled: true,
          },
        },
      },
    },
  ],
};
```


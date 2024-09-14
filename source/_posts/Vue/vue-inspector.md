---
title: vite-plugin-vue-inspector
date: 2024-09-14 14:56:05
typora-root-url: ../
cover: /images/cover/vue.png
top_img: false
tags: Vue
categories:
  - Vue
  - Vite
---

现在很多web项目都是基于前后端分离的，绝大多数都是基于`Vue`，`React`等框架进行开发的。现在的web项目较以前也日益复杂，每一个前端工程都有很多组件但是我们在进行开发的时候总是在`IDE`和页面来回切换，在页面上也不好定位自己需要改哪一个组件，基于此推荐一个vue的vite插件能很好的解决从页面上定位组件行数的问题。

## [vite-plugin-vue-inspector](https://github.com/webfansplz/vite-plugin-vue-inspector)

一个vite插件，提供点击浏览器元素自动跳转到本地IDE的功能。它支持 Vue2 & 3 & SSR。

![preview](/../assets/preview.gif)

#### Configuration Vite 配置Vite

```ts
// for Vue2
// vite.config.ts
import { defineConfig, } from 'vite'
import { createVuePlugin, } from 'vite-plugin-vue2'
import Inspector from 'vite-plugin-vue-inspector'

export default defineConfig({
  plugins: [
    createVuePlugin(),
    Inspector({
      vue: 2
    }),
  ],
})
```

```ts
// for Vue3
// vite.config.ts
import { defineConfig } from 'vite'
import Vue from '@vitejs/plugin-vue'
import Inspector from 'vite-plugin-vue-inspector'

export default defineConfig({
  plugins: [Vue(), Inspector()],
})
```

```ts
// for Nuxt3
// nuxt.config.ts
import { defineNuxtConfig } from 'nuxt/config'
import Inspector from 'vite-plugin-vue-inspector'

export default defineNuxtConfig({
  modules: [
    ['unplugin-vue-inspector/nuxt', {
      enabled: true,
      toggleButtonVisibility: 'always',
    }],
  ],
})
```

如果是`React`项目可以使用`LocatorJS`的谷歌商店插件 [传送门](https://chromewebstore.google.com/detail/locatorjs/npbfdllefekhdplbkdigpncggmojpefi?hl=zh-CN&utm_source=ext_sidebar&pli=1)。

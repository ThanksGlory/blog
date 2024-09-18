---
title: WindowInfo
date: 2024-09-18 13:39:00
typora-root-url: ../
cover: /images/cover/wechat.jpg
top_img: false
tags: WeChat
categories:
  - WeChat
---

有时候在开发小程序的时候会获取设备窗口信息，但是有些时候我们获取到的statusBarHeight为零，不知道是微信官方的BUG还是手机原因。出于该原因我们还是需要兼容一下写法：

### wx.getWindowInfo

| 属性            | 类型     | 说明                                                         |
| :-------------- | :------- | :----------------------------------------------------------- |
| pixelRatio      | number   | 设备像素比                                                   |
| screenWidth     | number   | 屏幕宽度，单位px                                             |
| screenHeight    | number   | 屏幕高度，单位px                                             |
| windowWidth     | number   | 可使用窗口宽度，单位px                                       |
| windowHeight    | number   | 可使用窗口高度，单位px                                       |
| statusBarHeight | number   | 状态栏的高度，单位px                                         |
| safeArea        | SafeArea | 在竖屏正方向下的安全区域。部分机型没有安全区域概念，也不会返回 safeArea 字段，开发者需自行兼容。 |
| screenTop       | number   | 窗口上边缘的y值                                              |

### safeArea

| 结构属性 | 说明   | 说明                         |
| :------- | :----- | ---------------------------- |
| left     | number | 安全区域左上角横坐标，单位px |
| right    | number | 安全区域右下角横坐标，单位px |
| top      | number | 安全区域左上角纵坐标，单位px |
| bottom   | number | 安全区域右下角纵坐标，单位px |
| width    | number | 安全区域的宽度，单位逻辑像素 |
| height   | number | 安全区域的高度，单位逻辑像素 |

### 图解以上属性

![safearea](/../assets/safearea.png)

### 适配代码

苹果设备的状态栏一般高度为 44px。在某些情况下，比如在 iPhone X 及之后的型号上，由于刘海设计，状态栏的高度可能会有所变化，但通常仍然保持在 44px 的范围内。

安卓手机的状态栏高度通常为 24dp，这在不同设备上可以转换为不同的像素值，取决于屏幕的密度（DPI）。在常见的屏幕密度下，24dp 大约相当于 24px 到 48px 之间。不过，这个高度可能会因设备的设计和版本而有所不同。通常建议参考具体设备的设计规范以获取准确的信息。

所以两者取平均值：IOS状态栏取`44px`，Android状态栏取`(24 + 48) / 2 = 36px`

```javascript
export default class SizeInfo {

  // 客户端平台
  static platform = "";

  // 窗口信息
  static windowInfo = null;

  // 获取窗口信息
  static getWindowInfo() {
    if (this.windowInfo) return this.windowInfo;
    let windowInfo = wx.getWindowInfo();
    if (this.isAndroid()) {
      windowInfo = this.formatWindowInfo(36, windowInfo);
    } else if (this.isIOS()) {
      windowInfo = this.formatWindowInfo(44, windowInfo);
    }
    this.windowInfo = windowInfo;
    return this.windowInfo;
  }

  // 格式化尺寸
  static formatWindowInfo(statusBarHeight = 36, windowInfo) {
    
    // 状态栏高度没获取到时候处理
    if (windowInfo.statusBarHeight === 0) {
      windowInfo.statusBarHeight = statusBarHeight;
      if (windowInfo.safeArea) {  
        windowInfo.safeArea.top = statusBarHeight;
      }
    }

    // 手机机型没有安全区时候适配
    if (!windowInfo.safeArea) {
      windowInfo.safeArea = {
        height: windowInfo.screenHeight - windowInfo.statusBarHeight,
        width: windowInfo.screenWidth,
        top: windowInfo.statusBarHeight,
        right: windowInfo.screenWidth,
        bottom: windowInfo.screenHeight,
        left: 0,
      }
    }
    return windowInfo;
  }

  // 获取设备基础信息
  static getPlatform() {
    if (!this.platform) {
      const platform = wx.getDeviceInfo().platform || '';
      this.platform = platform.toLocaleUpperCase();
    }
    return this.platform;
  }

  // 是否是IOS
  static isIOS() {
    const platform = this.getPlatform();
    return platform === 'IOS';
  }

  // 是否是ANDROID
  static isAndroid() {
    const platform = this.getPlatform();
    return platform === 'ANDROID';
  }

  // 是否是WINDOWS
  static isWindows() {
    const platform = this.getPlatform();
    return platform === 'WINDOWS';
  }

  // 是否是MAC
  static isMac() {
    const platform = this.getPlatform();
    return platform === 'MAC';
  }

  // 是否是DEVTOOLS
  static isDevtools() {
    const platform = this.getPlatform();
    return platform === 'DEVTOOLS';
  }
}
```


---
title: Flutter-开发准备- vscode 配置
date: 2022-09-04 17:26:21
typora-root-url: ../
cover: /images/cover/flutter.png
top_img: false
tags: Flutter

categories:
  - Flutter
  - flutter基础
---

# vscode 配置

## 打开配置文件

命令行模式下 `ctrl + shift + p`

![20220608155410](/assets/20220608155410.png)

## 配置 shell 下打开 code

输入 install code 搜索

![20220608155834](/assets/20220608155834.png)

## 自动应用提示

特别是大量的 `const` 提示警告

```json
  "editor.codeActionsOnSave": {
    "source.fixAll": true // 自动修复 all
  },
```

## 折叠配置文件

这样资源管理器文件列表就干净了

```json
  "explorer.fileNesting.enabled": true,
  "explorer.fileNesting.patterns": {
    "pubspec.yaml": ".packages, pubspec.lock, .flutter-plugins, .flutter-plugins-dependencies, .metadata, analysis_options.yaml, dartdoc_options.yaml"
  },
```

## 光标快速移动

设置键盘 “按键重复” “重复前延迟” ，这样打字快

![image-20220617061902456](/assets/image-20220617061902456.png)

命令行中执行，关闭 vscode 苹果长按提示

```shell
$ defaults write com.microsoft.VSCode ApplePressAndHoldEnabled -bool false
$ defaults write com.microsoft.VSCodeInsiders ApplePressAndHoldEnabled -bool false
```

## 括号线条高亮

全局设置

```shell
@id:editor.bracketPairColorization.enabled @id:editor.guides.bracketPairs
```

![image-20220620223132333](/assets/image-20220620223132333.png)

![image-20220620223345020](/assets/image-20220620223345020.png)

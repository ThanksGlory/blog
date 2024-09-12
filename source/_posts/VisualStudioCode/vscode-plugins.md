---
title: Visual Studio Code plugins
date: 2022-01-20 21:44:07
tags: Visual Studio Code
cover: /images/cover/vscode.png
top_img: false
typora-root-url: ../
categories:
  - Visual Studio Code
  - vscode 插件
---

### Todo Tree

此扩展快速搜索（使用[ripgrep](https://github.com/BurntSushi/ripgrep)）您的工作区以查找 TODO 和 FIXME 等注释标签，并将它们显示在活动栏中的树视图中。可以将视图从活动栏拖到资源管理器窗格中（或您希望的任何其他位置）。单击树中的 TODO 将打开文件并将光标放在包含 TODO 的行上。找到的 TODO 也可以在打开的文件中突出显示。

![screenshot](/../assets/screenshot.png)

### TODO Highlight

Highlight和代码`TODO`中`FIXME`的其他注释。

有时，在将代码发布到生产环境之前，您会忘记查看在编码时添加的 TODO。所以我一直想要一个扩展来突出它们并提醒我还有笔记或尚未完成的事情。希望这个扩展也能帮助你。

*注意*许多人报告该`List highlighted annotations`命令不起作用，请确保您通过`todohighlight.include`.

![image-20220120220935001](/../assets/image-20220120220935001.png)

### Git Graph

查看存储库的 Git 图表，并从图表轻松执行 Git 操作。可配置为您想要的方式！

![demo](/../assets/demo.gif)

### Chinese (Simplified) (简体中文)

通过使用“Configure Display Language”命令显式设置 VS Code 显示语言，可以替代默认 UI 语言。 按下“Ctrl+Shift+P”组合键以显示“命令面板”，然后键入“display”以筛选并显示“Configure Display Language”命令。按“Enter”，然后会按区域设置显示安装的语言列表，并突出显示当前语言设置。选择另一个“语言”以切换 UI 语言。 请参阅[文档](https://go.microsoft.com/fwlink/?LinkId=761051)并获取更多信息。

### vscode-icons

将图标带入您的[Visual Studio 代码](https://code.visualstudio.com/)（**支持的最低版本`1.40.2`**：）

![screenshot](/../assets/screenshot.gif)

### Live Server

![vscode-live-server-animated-demo](/../assets/vscode-live-server-animated-demo.gif)

### Document This

![demo](/../assets/demo-16426884665242.gif)

### Auto Close Tag

自动添加 HTML/XML 关闭标记，与 Visual Studio IDE 或 Sublime Text 相同。

### Auto Rename Tag

自动重命名成对的 HTML/XML 标记，与 Visual Studio IDE 相同。

### Quokka.js

安装此[扩展](https://link.segmentfault.com/?enc=6hNRBznSJUcx2%2BeNwS7akg%3D%3D.A4U1v13EhQ5M0r%2BlWH0vGvz20e15g6OUcJhU1umWsueC91f3qpxpemGi%2FFcmCWlIqyF%2FKwXcLU5BCjSYDDwJ76PbDAB1pB6kiCGB5lkbLKI%3D)后，可以按Ctrl / Cmd（⌘）+ Shift + P显示编辑器的命令选项板，然后键入 Quokka 以查看可用命令的列表。选择并运行 “New JavaScript File”命令。你也可以按（⌘+ K + J）直接打开文件。在此文件中输入的任何内容都会立即执行。

![demo](/../assets/demo-16426928874311.gif)

### Bracket Pair Colorizer

每个人都喜欢对代码着色，[Bracket Pair Colorizer](https://link.segmentfault.com/?enc=ofMCNLyzKvbEysNH6R2%2FQw%3D%3D.wxbBqAqUd9hdMMyjeKbp2y3%2BTniF4J3O4%2BHduXqW5p3CQaiRppHaIAvvQiIn0NGewWQL%2B%2FK6VYOfGg1vJWYiRP%2BM10mhyb6cgpDcj0yzYtNhv86u0AAp1agpfpoBl5UR)提供了匹配颜色的左括号和右括号，从而更容易知道哪些括号属于谁。

还可以配置自定义括号字符，你也可以为活动范围添加背景颜色。

![bracket_pair_colorizer](/../assets/bracket_pair_colorizer.png)

### Turbo Console Log

自动创建有意义的日志消息，该[控制台显示日志](https://link.segmentfault.com/?enc=JVuF%2BH3i8%2F4ueCHbORbfMA%3D%3D.POZA7fxykEkwZtA5qGRs9%2FKEiUjHKQN5vlo0P8ls9KBHA6rw06f%2Bbnapd%2FdAegoI5o%2F6NANV73WESPnDmPm%2BM9UOr2vmOEgcTbdkNJnx%2BitxNi8rwNAiOmYLS1fvMju1)\插件自动创建一个有意义的日志信息的过程。它使调试更容易，因为你将有一些有用的控制台输出来找出问题所在。

![turbo_console_log](/../assets/turbo_console_log.gif)

### Regex Previewer

创建正则表达式的预览，正则表达式可能是一个很困难的难题。[Regex Previewer](https://link.segmentfault.com/?enc=gfHYLOmk7kWcgdxd5OEw0w%3D%3D.GxBC%2Bh11m0hd2GUmtDyBVR2DLwLittDyQA4om4twMyYr4XjTb4oTm8Nmy6mJOfko%2BoIjjkPoDK2lPwjoPSNQ%2Fdb0kWtDW%2FdiYlQMqm0e4Ms%3D)为你提供与你的正则表达式匹配的辅助文档。

该插件提供了多个示例进行匹配，因此为各种用例快速准确地编写正则表达式变得更加容易

![regex_previewer](/../assets/regex_previewer.gif)

### Bookmarks

为你的代码添加书签，尽管 VSCode 有行号，但[Bookmarks](https://link.segmentfault.com/?enc=Uugnew%2BGZCTGT6N%2BovwC%2BQ%3D%3D.IZfACruiCvQspp6sPfrlVmv4mwGmcGL%2FBP3hXEN%2F0y%2BdkrSa%2FVWxMnHVX9n1vbB5XL9detkzM72gOTSDoyD2qRwfaoW0dMzbS6pZyC5nThs%3D)允许你在代码中添加书签，帮助你快速导航并轻松来回跳转。

![Bookmarks](/../assets/Bookmarks.png)

### HTML/CSS/JavaScript Snippets

只需输入前缀名称，就会自动完成完整的代码片段。

### Javascript Code Snippets

提供很多 JS 代码块提示，虽然 VSCode 包括内置的 JS IntelliSense，但[JS 代码片段插件](https://link.segmentfault.com/?enc=mFOjADr7Vno3leQ2NKsYCw%3D%3D.zWnYC7LsFoU%2Bh2ElMK6vav9vSzKXnQtVVLSmEufc4L9x2ncjQoqOu2wZk2gSj0czMxdM6HRc8ca9SRBaTtIM5vMZMvVuWuSsIvxzggBpk00%3D)通过添加大量导入、导出触发器、类助手和方法触发器来增强这种体验。

该插件支持 JS、TypeScript、JS React、TS React、HTML 和 Vue。在 VSCode Marketplace 中，也可以轻松获得其他风格（例如 Angular）的代码片段。

![Javascript_Code_Snippets](/../assets/Javascript_Code_Snippets.png)

### CSS Peek

插件你的 HTML 文件以查看你的 CSS 代码

这个插件对于前端开发人员来说是无价的。受 IDE Brackets 中类似功能的启发，[CSS Peek](https://link.segmentfault.com/?enc=%2B0UQVBSg5NA3%2BX0C2P8IHA%3D%3D.nQtCHn54OZBj%2Fgf2HLrgLTezje2xb54AlrxZoyigFxOs18PKvPoi1an%2F7Z3R2k5d2U6cxHQTJBMzbfw%2BYi2jpmH%2FHlxp3Ytn%2FMF%2B7GyBkoc%3D)允许你插件 HTML 和 ejs 文件以在源代码中显示 CSS/SCSS/LESS 代码。

如果你知道类或 ID 名称，它还允许你快速跳转到对应 CSS 代码的位置。

![CSS_Peek](/../assets/CSS_Peek.gif)

### Svg Preview

现在图标基本上都是 svg 能预览还是很重要

https://marketplace.visualstudio.com/items?itemName=SimonSiefke.svg-preview

![20220608141120](/../assets/20220608141120.png)

### Path Intellisense

自动完成文件路径

https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense

![20220608153647](/../assets/20220608153647.png)

### GitHub Theme

随着环境不同，你可以切换配色来让眼睛舒服。

https://marketplace.visualstudio.com/items?itemName=GitHub.github-vscode-theme

![20220608140649](/../assets/20220608140649.png)

![20220608140656](/../assets/20220608140656.png)

### Material Icon Theme

通过图标快速识别文件作用

https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme

![20220608140942](/../assets/20220608140942.png)

### Comment Translate

注释翻译

https://marketplace.visualstudio.com/items?itemName=intellsmi.comment-translate

![image-20220621113346445](/../assets/image-20220621113346445.png)

### Iconify IntelliSense

前端图标识别插件 iconfont 系列，能更直观的查看该图标结构。

![Iconify_IntelliSense](/../assets/Iconify_IntelliSense.png)


<center><font size="30">Flutter开发插件</font></center>

### Flutter

https://marketplace.visualstudio.com/items?itemName=Dart-Code.flutter

![20220617160834](/../assets/20220617160834.png)

### Dart

https://marketplace.visualstudio.com/items?itemName=Dart-Code.dart-code

![dart_plugins](/../assets/dart_plugins.png)

### Awesome Flutter Snippets

代码片段快捷

https://marketplace.visualstudio.com/items?itemName=Nash.awesome-flutter-snippets

![20220608140434](/../assets/20220608140434.png)

### Flutter Widget Snippets

又一个代码片段快捷

https://marketplace.visualstudio.com/items?itemName=alexisvt.flutter-snippets

![20220608141304](/../assets/20220608141304.png)

### Json to Dart Model

根据 json 数据格式自动生成 entity 类

https://marketplace.visualstudio.com/items?itemName=hirantha.json-to-dart

![20220608140815](/../assets/20220608140815.png)

### GetX Snippets

getx 代码提示

https://marketplace.visualstudio.com/items?itemName=get-snippets.get-snippets

![20220608144324](/../assets/20220608144324.png)

### Flutter GetX Generator - 猫哥

猫哥写的插件，很多快捷方式，非常实用

https://marketplace.visualstudio.com/items?itemName=ducafecat.getx-template

![20220608144436](/../assets/20220608144436.png)

### Visual Studio Code 配置

```json
{
    // 编辑器字体
    "editor.fontFamily": "'Fira Code', '微软雅黑',Consolas, 'Courier New', monospace",
    "editor.fontLigatures": false,
    "editor.fontSize": 16,
    "editor.lineHeight": 22,
    // "editor.letterSpacing": 0.5,
    "editor.wordWrap": "off",
    "files.trimTrailingWhitespace": true,
    "editor.fontWeight": "450",
    "editor.cursorStyle": "line",
    "editor.cursorWidth": 3,
    "editor.cursorBlinking": "solid",
    "security.workspace.trust.untrustedFiles": "open",
    "editor.mouseWheelZoom": true,
    "backgroundCover.imagePath": "f:\\pictures\\123036.jpg",
    "backgroundCover.opacity": 0.05,
    "workbench.iconTheme": "vscode-icons",
    "less.scannerExclude": [
        "**/.git",
        "**/node_modules",
        "**/bower_components",
        "**/public"
    ],
    "editor.codeActionsOnSave": {
        "source.fixAll": true // 自动修复 all
    },
    "explorer.fileNesting.enabled": true,
    "explorer.fileNesting.patterns": {
        "pubspec.yaml": ".packages, pubspec.lock, .flutter-plugins, .flutter-plugins-dependencies, .metadata, analysis_options.yaml, dartdoc_options.yaml"
    },
    "editor.guides.bracketPairs": true,
    "redhat.telemetry.enabled": true,
    "editor.unicodeHighlight.allowedLocales": {
        "tr": true
    },
    "[vue]": {
        "editor.defaultFormatter": "Vue.volar"
    },
    "json.schemaDownload.enable": false,
    "yaml.schemaStore.enable": false,
    "dart.showInspectorNotificationsForWidgetErrors": false,
    "dart.debugExternalPackageLibraries": true,
    "dart.debugSdkLibraries": true,
    "vsicons.dontShowNewVersionMessage": true,
    "workbench.colorTheme": "Default Dark+",
    "editor.tabSize": 2,
}
```


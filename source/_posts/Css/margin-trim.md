---
title: margin trim
date: 2024-09-12 17:03:44
top_img: false
cover: /images/cover/flex.png
comments: true

tags: margin
categories:
  - Css
typora-root-url: ../
---

在项目中布局平常都会遇到一些排版上的问题,，比如垂直排列的元素之间增加下外边距：

![PixPin_2024-09-12_17-15-06](/../assets/PixPin_2024-09-12_17-15-06.png)

我们需要li之间出现margin，而不是1，2，3都有margin，通常做法是nth-last-child去掉最后一个元素的margin-bottom，但是有点麻烦，下面介绍一种方式只需一行css即可解决上面的问题，代码如下所示：

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    ul {
      border: 1px solid #000;
    }

    li+li {
      margin-top: 50px;
    }
  </style>
</head>

<body>
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
  </ul>
</body>

</html>
```

使用 ` li + li` 的方式解决上面布局问题，针对li的后一个兄弟元素进行设置样式，则第一个元素就不起作用，效果如下图所示。

![PixPin_2024-09-12_17-20-48](/../assets/PixPin_2024-09-12_17-20-48.png)

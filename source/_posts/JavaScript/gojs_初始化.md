---
title: gojs基础教程
date: 2025-09-24 21:44:13
cover: /images/cover/gojs.png
top_img: false
tags: JavaScript
categories:
  - JavaScript
  - gojs
---

### 一、初始化

#### gojs的引包是怎么写的？

用的go-debug.js

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/gojs/1.7.2/go-debug.js"></script> 
```

#### gojs的初始化是怎么写的？

go的**GraphObject**属性的make属性

```js
var $ = go.GraphObject.make;
```

#### gojs的配参数（json）是怎么写的？

```js
var myDiagram = $(go.Diagram, "myDiagramDiv",{
  initialContentAlignment: go.Spot.Center, // center Diagram contents
  "undoManager.isEnabled": true // enable Ctrl-Z to undo and Ctrl-Y to redo
});
```

#### gojs的引数据（json）是怎么写的？

```js
var myModel = $(go.Model);
// in the model data, each node is represented by a JavaScript object:
myModel.nodeDataArray = [
  { key: "Alpha" },
  { key: "Beta" },
  { key: "Gamma" }
];
myDiagram.model = myModel;
```



### 二、GoJS入门

GoJS 是一个实现交互式图表(Diagram)的Javascript 库。这个页面展示了使用GoJS的精髓。 因为GoJS是一个依赖于HTML5特性的JavaScript库，所以你要搞清楚浏览器是否支持HTML5，当然首先要加载这个库；

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>gojs</title>
  <!-- use go-debug.js when developing and go.js when deploying -->
  <script src="go-debug.js"></script>
</head>
<body>
</body>
</html>
```

你可以在[这里](http://gojs.net/latest/doc/download.html)下载,也可以直接引用这个地址[CDN](https://cdnjs.com/libraries/gojs):

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/gojs/1.7.2/go-debug.js"></script>
```

每个GoJS图表包含在一个页面中固定尺寸的HTML的元素中：

```html
<!-- The DIV for a Diagram needs an explicit size or else we will not see anything. In this case we also add a background color so we can see that area. -->
<div id="myDiagramDiv" style="width:400px; height:150px; background-color: #DAE4E4;"></div>
```

在Javascript代码中你可以传递div的id来创建一个图表(Diagram):

```js
var $ = go.GraphObject.make;
var myDiagram = $(go.Diagram, "myDiagramDiv");
```
![ee059240493b](/assets/ee059240493b.png)

记住GoJS的命名空间是 go，所有的gojs包含的类型都在go这个命名空间下。所有的gojs的类比如Diagram 、Node 、 Panel 、 Shape 、 TextBlock 都是以go.为前缀的。

这篇文章会举例告诉你如何调用go.GraphObject.make来创建一个GoJS对象。更详细的介绍情况请看 [Building Objects in GoJS](http://gojs.net/latest/intro/buildingObjects.html)，它使用$作为go.GraphObject.make的缩写，这样很方便。如果你的代码中$表示别的对象，你也可以选一个其它的短变量，比如$$或者MAKE或者go.

#### 图表和模型

图表(Diagram)的节点(Node)和链接(Link)是模型管理下的可视数据。gojs支持模型-视图构架，当模型包含描述节点和链接的数据（Javascript的数组对象）。而且图表使用视图的方式来表现数据中的节点和链接对象。我们加载、编辑、保存的是模型而不是图表。你在模型的数据对象中添加你的APP需要的属性，而不是去修改Diagram 和GraphObject 类的原型。下面是一个模型和图表(Diagram)的例子，再下面就是这个例子生成的图：

```javascript
var $ = go.GraphObject.make;
var myDiagram =
  $(go.Diagram, "myDiagramDiv",
    {
      initialContentAlignment: go.Spot.Center, // center Diagram contents
      "undoManager.isEnabled": true // enable Ctrl-Z to undo and Ctrl-Y to redo
    });

var myModel = $(go.Model);
// in the model data, each node is represented by a JavaScript object:
myModel.nodeDataArray = [
  { key: "Alpha" },
  { key: "Beta" },
  { key: "Gamma" }
];
myDiagram.model = myModel;
```

![cde902ed4081](/assets/cde902ed4081.png)

图表(Diagram)展示了模型的三个节点。也能够实现一些交互：

- 点击和拖放背景图表一直跟着移动.
- 通过点击选择一个节点，也可以拖放节点.
- 为了创建一个选择框，直接点击握住背景，然后开始拖动就可以了。
- 使用Ctrl-C和Ctrl-V或者通过拖放来实现拷贝和粘贴.
- 因为可以使用撤销(undo)管理器，Ctrl-Z是撤销，Ctrl-Y是重做

#### 节点风格

节点的风格是由GraphObjects的模板和对象的属性值决定的，为了创建一个节点Node，我们有一些构建形状块(building block)的类：**Shape**, 显示预定义或者定制的带颜色的几何图形。**TextBlock**, to display (potentially editable) text in various fonts。**Picture**, 显示图片。**Panel**,一个包含了其它的对象集合的容器，可以根据Panel的类型修改位置、尺寸。

所有的构建形状块(building block)都是从GraphObject 这个抽象类继承过来的, 所以我们可以随意的以GraphObjects 的方式引用他们。注意GraphObjects 不是一个HTML DOM 元素，所以不需要创建或者修改此类元素。

我们想通过模型数据的属性来影响节点，且这个是通过数据绑定来实现的。数据绑定允许我们修改节点上的GraphObjects的属性来修改 的外观和行为。模型对象是Javascript对象。你可以选择任意你想用的属性名来定义模型。缺省的节点模板是非常简单的，一个节点包含一个TextBlock类.TextBlock的Text属性和模型的key要绑定到一起，就像这样的代码：

```js
myDiagram.nodeTemplate =
  $(go.Node,
    $(go.TextBlock,
      // TextBlock.text is bound to Node.data.key
      new go.Binding("text", "key"))
  );
```

注意没有Node.key的属性。但是你可以通过someNode.data.key来获取Node的key值。

TextBlocks,Shapes和Pictures是GoJS的原始的构建形状块。TextBlocks不能包含图片，Shapes不能包含文字。如果你想要节点显示文字，必须用TextBlock。如果你想画或者填充一些几何图形，你必须使用Shape.

一般情况下，节点的模块代码就像下面这个样子：

```js
myDiagram.nodeTemplate =
  $(go.Node, "Vertical" // second argument of a Node/Panel can be a Panel type
    /* set Node properties here */
    { // the Node.location point will be at the center of each node
      locationSpot: go.Spot.Center
    },

    /* add Bindings here */
    // example Node binding sets Node.location to the value of Node.data.loc
    new go.Binding("location", "loc"),

    /* add GraphObjects contained within the Node */
    // this Shape will be vertically above the TextBlock
    $(go.Shape,
      "RoundedRectangle", // string argument can name a predefined figure
      { /* set Shape properties here */ },
      // example Shape binding sets Shape.figure to the value of Node.data.fig
      new go.Binding("figure", "fig")),

    $(go.TextBlock,
      "default text",  // string argument can be initial text string
      { /* set TextBlock properties here */ },
      // example TextBlock binding sets TextBlock.text to the value of Node.data.key
      new go.Binding("text", "key"))
  );
```

Panels里面的GraphObjects 可以附着在任意的深度，而去每个类有他自己独有的属性，但是这里展示的是一般情况。现在我们已经看了如果创建一个节点模板，让我们看一个实际的例子。我梦将会创建一个简单的组织架构图-节点包含名字和图片。考虑下如下的节点模板：一个"水平"类型的节点，意思是节点中的元素将会水平并排放置，它有两个元素：一个Picture对象表示照片，包含一个图片源数据的绑定一个TextBlock对象表示名字，包含文字数据的绑定。

```js
var $ = go.GraphObject.make;
var myDiagram =
  $(go.Diagram, "myDiagramDiv",
    {
      initialContentAlignment: go.Spot.Center, // center Diagram contents
      "undoManager.isEnabled": true // enable Ctrl-Z to undo and Ctrl-Y to redo
    });

// define a simple Node template
myDiagram.nodeTemplate =
  $(go.Node, "Horizontal",
    // the entire node will have a light-blue background
    { background: "#44CCFF" },
    $(go.Picture,
      // Pictures should normally have an explicit width and height.
      // This picture has a red background, only visible when there is no source set
      // or when the image is partially transparent.
      { margin: 10, width: 50, height: 50, background: "red" },
      // Picture.source is data bound to the "source" attribute of the model data
      new go.Binding("source")),
    $(go.TextBlock,
      "Default Text",  // the initial value for TextBlock.text
      // some room around the text, a larger font, and a white stroke:
      { margin: 12, stroke: "white", font: "bold 16px sans-serif" },
      // TextBlock.text is data bound to the "name" attribute of the model data
      new go.Binding("text", "name"))
  );

var model = $(go.Model);
model.nodeDataArray =
[ // note that each node data object holds whatever properties it needs;
  // for this app we add the "name" and "source" properties
  { name: "Don Meow", source: "cat1.png" },
  { name: "Copricat", source: "cat2.png" },
  { name: "Demeter",  source: "cat3.png" },
  { /* Empty node data */  }
];
myDiagram.model = model;
```

以上的代码产生了下面这个图表：

![64fff1d53be5](/assets/64fff1d53be5.png)

当不是所有的信息都被完全展示时，我们可能会想显示一些缺省的的状态：比如图片没有下载完或者还不知道名字的时候。例子中空节点的数据就可以作为占位符。

#### 模型的种类

包含自定义模板的图表是非常漂亮的，但是或许我们想展示更多。比如我们想展示一个组织架构图，Don Meow的确是这个组织的老板。因此我们会创建一个完整的组织结构图，图中会在节点之间加上一些链接来展示人物关系，并且包含一个图层来自动排列节点。

为了把链接加入图中，基础的模型是不够用的。我们将会加入**GoJS**中的两个其它的模型。他们是GraphLinksModel和 TreeModel。(想要了解更多看 [这里](http://gojs.net/latest/intro/usingModels.html).)

在GraphLinksModel中, 我们除了model.nodeDataArray还有 model.linkDataArray。他包含了一个Javascript 对象数组，每个数组表示一个指定了“from”和“to”的链接的节点key。这是一个例子表示节点A链接到节点B以及节点B链接到节点C:

```js
var model = $(go.GraphLinksModel);
model.nodeDataArray =
[
  { key: "A" },
  { key: "B" },
  { key: "C" }
];
model.linkDataArray =
[
  { from: "A", to: "B" },
  { from: "B", to: "C" }
];
myDiagram.model = model;
```

一个GraphLinksModel 允许你任意个节点间的链接，包含任意的方向。可以有十个A到B的链接，和三个相反的B到A的链接。

一个TreeModel和GraphLinksModel有点不同。他并不包含一个链接数据的数组，而是创建了一个节点数据“父节点”的模型。链接用这种方式创建。下面是一个TreeModel的例子，包含一个节点A链接到B，B链接到C:

```js
var model = $(go.TreeModel);
model.nodeDataArray =
[
  { key: "A" },
  { key: "B", parent: "A" },
  { key: "C", parent: "B" }
];
myDiagram.model = model;
```

TreeModel比 GraphLinksModel简单,但是他不能随意创建链接关系，比如不能在两个节点间创建的多个链接，也不能有多个父节点。我们的组织架构图是简单树形层级结构，所以我们选择TreeModel来实现这个例子：

首先，我们添加 几个节点，然后用TreeModel来指定key和父节点：

```js
var $ = go.GraphObject.make;
var myDiagram =
  $(go.Diagram, "myDiagramDiv",
    {
      initialContentAlignment: go.Spot.Center, // center Diagram contents
      "undoManager.isEnabled": true // enable Ctrl-Z to undo and Ctrl-Y to redo
    });

// the template we defined earlier
myDiagram.nodeTemplate =
  $(go.Node, "Horizontal",
    { background: "#44CCFF" },
    $(go.Picture,
      { margin: 10, width: 50, height: 50, background: "red" },
      new go.Binding("source")),
    $(go.TextBlock, "Default Text",
      { margin: 12, stroke: "white", font: "bold 16px sans-serif" },
      new go.Binding("text", "name"))
  );

var model = $(go.TreeModel);
model.nodeDataArray =
[ // the "key" and "parent" property names are required,
  // but you can add whatever data properties you need for your app
  { key: "1",              name: "Don Meow",   source: "cat1.png" },
  { key: "2", parent: "1", name: "Demeter",    source: "cat2.png" },
  { key: "3", parent: "1", name: "Copricat",   source: "cat3.png" },
  { key: "4", parent: "3", name: "Jellylorum", source: "cat4.png" },
  { key: "5", parent: "3", name: "Alonzo",     source: "cat5.png" },
  { key: "6", parent: "2", name: "Munkustrap", source: "cat6.png" }
];
myDiagram.model = model;
```

![481fa8ad21bd](/assets/481fa8ad21bd.png)

#### 图表(Diagram)布局

正如你可以看到的，TreeModel自动创建需要的节点间的链接，但是它很难确定谁的父节点是谁。

图表(Diagram)有一个缺省布局会管理那些没有具体位置的节点，会自动分配一个位置。我们可以显示的给每个节点分配一个位置来给组织排序，但是在我们的例子中一个更容易的解决方案是，我们会使用布局来自动排列位置。

我们想要来显示一个层级，而且已经用了TreeModel，因此最自然的方式是使用TreeLayout。TreeLayout缺省的是从左到右，因此为了表示从上到下的我们会将angle属性设置为90.

在**GoJS**中使用布局通常很简单。每种类型的布局有一些属性能影响结果。每种布局都是有例子的：(比如[TreeLayout Demo](http://gojs.net/latest/samples/tLayout.html))

```js
// define a TreeLayout that flows from top to bottom
myDiagram.layout =
  $(go.TreeLayout,
    { angle: 90, layerSpacing: 35 });
    

GoJS 有许多其它的布局，你可以看 这里.
给图表添加布局，我们可以看到如下结果：
var $ = go.GraphObject.make;
var myDiagram =
  $(go.Diagram, "myDiagramDiv",
    {
      initialContentAlignment: go.Spot.Center, // center Diagram contents
      "undoManager.isEnabled": true, // enable Ctrl-Z to undo and Ctrl-Y to redo
      layout: $(go.TreeLayout, // specify a Diagram.layout that arranges trees
                { angle: 90, layerSpacing: 35 })
    });

// the template we defined earlier
myDiagram.nodeTemplate =
  $(go.Node, "Horizontal",
    { background: "#44CCFF" },
    $(go.Picture,
      { margin: 10, width: 50, height: 50, background: "red" },
      new go.Binding("source")),
    $(go.TextBlock, "Default Text",
      { margin: 12, stroke: "white", font: "bold 16px sans-serif" },
      new go.Binding("text", "name"))
  );

var model = $(go.TreeModel);
model.nodeDataArray =
[
  { key: "1", name: "Don Meow", source: "cat1.png" },
  { key: "2", parent: "1", name: "Demeter",    source: "cat2.png" },
  { key: "3", parent: "1", name: "Copricat",   source: "cat3.png" },
  { key: "4", parent: "3", name: "Jellylorum", source: "cat4.png" },
  { key: "5", parent: "3", name: "Alonzo",     source: "cat5.png" },
  { key: "6", parent: "2", name: "Munkustrap", source: "cat6.png" }
];
myDiagram.model = model;
```

![692cb9ddcede](/assets/692cb9ddcede.png)

这个图表看起来像是组织机构那么回事，但是我们可以做得更好。

#### 链接模板(Link templates)

我们会构建一个新的链接模板（link template），这个模板可以更好的适应我们的宽的盒状的节点。链接[Link](http://gojs.net/latest/intro/links.html)是一个不同于Node的部分。链接主要的元素是链接的形状，而且必须是一个由 **GoJS**动态计算出来的形状。我们的链接是将会包含形状形状、比一般黑色的更宽一点的灰色笔画。不像缺省的链接模板，我们不会有箭头。而且我们会将Link routing 属性从Normal修改为Orthogonal，而且给他一个拐角值因此角会变园。

```js
// define a Link template that routes orthogonally, with no arrowhead
myDiagram.linkTemplate =
  $(go.Link,
    // default routing is go.Link.Normal
    // default corner is 0
    { routing: go.Link.Orthogonal, corner: 5 },
    $(go.Shape, { strokeWidth: 3, stroke: "#555" }) // the link shape

    // if we wanted an arrowhead we would also add another Shape with toArrow defined:
    // $(go.Shape, { toArrow: "Standard", stroke: null }
    );
```

综合我们的链接模板、节点模板、TreeModel和Treelayou，我们最终得到了一个完整的组织架构图。完整的代码在下面，紧跟的是生成的结果图：

```js
var $ = go.GraphObject.make;

var myDiagram =
  $(go.Diagram, "myDiagramDiv",
    {
      initialContentAlignment: go.Spot.Center, // center Diagram contents
      "undoManager.isEnabled": true, // enable Ctrl-Z to undo and Ctrl-Y to redo
      layout: $(go.TreeLayout, // specify a Diagram.layout that arranges trees
                { angle: 90, layerSpacing: 35 })
    });

// the template we defined earlier
myDiagram.nodeTemplate =
  $(go.Node, "Horizontal",
    { background: "#44CCFF" },
    $(go.Picture,
      { margin: 10, width: 50, height: 50, background: "red" },
      new go.Binding("source")),
    $(go.TextBlock, "Default Text",
      { margin: 12, stroke: "white", font: "bold 16px sans-serif" },
      new go.Binding("text", "name"))
  );

// define a Link template that routes orthogonally, with no arrowhead
myDiagram.linkTemplate =
  $(go.Link,
    { routing: go.Link.Orthogonal, corner: 5 },
    $(go.Shape, { strokeWidth: 3, stroke: "#555" })); // the link shape

var model = $(go.TreeModel);
model.nodeDataArray =
[
  { key: "1", name: "Don Meow", source: "cat1.png" },
  { key: "2", parent: "1", name: "Demeter",    source: "cat2.png" },
  { key: "3", parent: "1", name: "Copricat",   source: "cat3.png" },
  { key: "4", parent: "3", name: "Jellylorum", source: "cat4.png" },
  { key: "5", parent: "3", name: "Alonzo",     source: "cat5.png" },
  { key: "6", parent: "2", name: "Munkustrap", source: "cat6.png" }
];
myDiagram.model = model;
```

![9c38340700a7](/assets/9c38340700a7.png)

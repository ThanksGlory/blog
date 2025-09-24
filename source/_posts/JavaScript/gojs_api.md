---
title: GoJs常用API
date: 2025-09-24 22:35:09
cover: /images/cover/gojs.png
top_img: false
tags: JavaScript
categories:
  - JavaScript
  - gojs
---

### GoJs常用API

#### 添加节点

myDiagram.model.addNodeData(node);

```js
var node = {};
node["key"] = "节点Key";
node["loc"] = "0 0";//节点坐标
node["text"] = "节点名称";
myDiagram.model.addNodeData(node);
```

应用场景：通过按钮点击，添加新的节点到画布。

#### 删除选中节点

myDiagram.commandHandler.deleteSelection();

```js
if (myDiagram.commandHandler.canDeleteSelection()) {
    myDiagram.commandHandler.deleteSelection();
    return;
}
```

应用场景：页面上有个按钮点击，可以删除选择的节点和线

#### 获取当前画布的json

myDiagram.model.toJson()

```js
var model= myDiagram.model.toJson();获得整个画布的json
// model=eval('('+model+')');若格式异常抓一下
var nodes=model.nodeDataArray;取出所有节点
var Links=model.linkDataArray;取出所有线
```

应用场景：获取当前画布的所有元素的json，用来保存。

#### 加载json刷新画布

myDiagram.model = go.Model.fromJson(model);

```js
model=myDiagram.model.toJson()
myDiagram.model = go.Model.fromJson(model);
```

应用场景：一般用来刷新和加载画布上的元素。

#### 通过节点key获取节点

myDiagram.findNodeForKey('key');

```js
var node = myDiagram.findNodeForKey('key');
```

应用场景：知道节点key ，拿到这个节点的详细信息。

#### 更改节点属性值

myDiagram.model.setDataProperty(node, 'color', "#ededed");

```js
var node = myDiagram.model.findNodeDataForKey('key');  //首先拿到这个节点的对象
myDiagram.model.setDataProperty(node, 'color', "#ededed");  //然后对这个对象的属性进行更改
```

应用场景：更改节点的颜色，或者大小等。

#### 获取获得焦点的第一个元素，可为节点或者线

myDiagram.selection.first();

```js
var node = myDiagram.selection.first();
console.log(node.data.key);
```

#### 获取所有获得焦点的节点

myDiagram.nodes

```js
var items = '';
for (var nit = myDiagram.nodes; nit.next();) {
  var node = nit.value;
  if (node.isSelected) {
    items += node.data.key + ",";
  }
}
console.log(items);
```

#### 遍历整个画布的节点信息写法1

```js
for (var nit = myDiagram.nodes; nit.next();) {
  var node = nit.value;
  var key = node.data.key;
  var text = node.data.text;
}
```

#### 给节点添加线

```js
myDiagram.model.addLinkData({ from: "节点的Key", to: "连到另一节点的key" })
```

#### 选中节点

```js
var newdata = { "text": "AAAA", "key": -33, "loc": "0 0" };
myDiagram.model.addNodeData(newdata);
var newdat2 = myDiagram.model.findNodeDataForKey('-3');
console.log(newdat2);
var node = myDiagram.findNodeForData(newdat2);
console.log(node);
myDiagram.select(node); //选中节点
```

#### 特殊案例API用法

myDiagram.findNodeForData(objNode)

myDiagram.findLinkForData(objLink)

```js
// 其中objNode或者objLink,只能从画布的json 对象取出，
// 不能直接手写例如
var newdata = { "text": "AAAA", "key": -33, "loc": "0 0" };
var node = myDiagram.findNodeForData(newdat2);
// 除了刚好是新建的节点外，不然是获取不到这个对象的，因为添加节点时，gojs会自动给节点或者线添加一个属性
```

### 常用事件定义API，和用法

#### 节点选中改变事件

selectionChanged: 回调的函数方法名

该属性要写在$(go.Node,）内用大括号括起来，如下面例子

```js
myDiagram.nodeTemplate =
  $(go.Node, "Horizontal",
    { selectionChanged: nodeSelectionChanged },
    //节点选中改变事件，nodeSelectionChanged为回调的方法名
    $(go.Panel, "Auto",
      $(go.Shape,//节点形状和背景颜色的设置
        { fill: "#1F4963" },
        new go.Binding("fill", "color")
      ),
    )
  );//go.Node的括号

//回调方法
function nodeSelectionChanged(node) {
  if (node.isSelected) {//
    //节点选中执行的内容
    console.log(node.data);//节点的属性信息
    console.log(node.data.key);//拿到节点的Key,拿其他属性类似
    var node1 = myDiagram.model.findNodeDataForKey(node.data.key);
    myDiagram.model.setDataProperty(node1, "fill", "#ededed");
  } else {
    //节点取消选中执行的内容
    var node1 = myDiagram.model.findNodeDataForKey(node.data.key);
    myDiagram.model.setDataProperty(node1, 'fill', "1F4963 ");
  }
}
// 节点选中的时候是一种颜色，取消选中是另一种颜色
```

#### 节点双击事件

```js
doubleClick : function(e, node){
  // node为当前双击的节点的对象信息
  // 该属性要写在$(go.node,）内用大括号括起来，如下面例子
}
```

```js
myDiagram.nodeTemplate =
  $(go.Node, "Horizontal",
    $(go.Panel, "Auto",
      $(go.Shape,//节点形状和背景颜色的设置
        { fill: "#1F4963" },
        new go.Binding("fill", "color")
      ),
      {
        doubleClick: function (e, node) {// 双击事件
          handlerDC(e, node);//双击执行的方法
        }
      }
    )
  );//go.Node的括号

//双击执行的方法
function handlerDC(e, obj) {
  var node = obj.part;//拿到节点的json对象，后面要拿什么值就直接.出来
  var procTaskId = node.data.key;
  var procTaskName = node.data.text;
  var description = node.data.description;
  var procTmplId = node.data.tmplId;
}
// 该例子主要应用场景为，双击节点，得到节点的详细信息，弹出窗口修改节点的信息，
```


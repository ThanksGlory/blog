---
title: JavaScript 执行机制
date: 2022-01-14 15:46:45
tags: JavaScript
cover: /images/cover/javascript_context.jpg
top_img: /images/cover/javascript_context.jpg
typora-root-url: ../
categories:
  - JavaScript
---

**<center><font size="5">JavaScript 执行机制</font></center>**

JavaScript是一种弱类型的解释语言，所以JavaScript在从一段代码到输出结果会经过两个阶段：

- 预编译阶段
- 执行阶段

以代码块为范围，即每遇到一个代码块都会进行  `预编译` ---> `执行`

#### 预编译阶段

在其他诸如 Java 语言中，程序的执行需要编译。JavaScript中也有类似的“预编译阶段”（JavaScript预编译到代码块`<script></script>`，即每一个代码块都会执行`预编译`---->`执行`），了解 JavaScript 引擎的实现机制，有助于总结 JS 代码过程中的思想。首先，JavaScript中的两个声明，`var`和`function`，前者声明变量，后者声明方法。

在预编译中，JavaScript 对这两条语句做了两种处理方案。

```js
var a = "1"; // Declare the variable a

function  b () {// declaration method b
  alert();
}
var c = function() {// declared variable c
  alert();
}
```

上述代码块中，a、c为赋值，b为函数声明，执行上述代码时，

首先进入预编译阶段，

对于赋值a，c会在内存中打开一个内存空间，指向内存中的变量名，并赋值为undefined

对于函数声明，也会打开内存空间，但是会直接处理函数体，即使用函数声明，在预编译阶段就已经完成了函数的创建。

- 预编译阶段**（PS：不管变量和声明函数的声明顺序，都是在预编译阶段先声明变量，再声明函数。所以通常的情况下函数和变量声明重复后函数会覆盖变量。）**

```js
var a = undefined;
var c = undefined;

var b = function(){
  alert();
}
```

- 执行阶段

```js
a = "1";
c = function(){
  alert();
}
```

以上代码执行步骤

```js
var a = undefined;
var c = undefined;

var b = function(){
  alert();
}
a = "1";
c = function(){
  alert();
}
```

exp：

```js
var a =1;
function test(){
  alert(a); // a为undefined! 这个a并不是全局变量，这是因为在function scope里已经声明了（函数体倒数第4行）一个重名的局部变量,所以全局变量a被覆盖了，这说明了Javascript在执行前会对整个脚本文件的定义部分做完整分析,所以在函数test()执行前,  函数体中的变量a就被指向内部的局部变量.而不是指向外部的全局变量. 但这时a只有声明，还没赋值，所以输出undefined。
  a = 4;
  alert(a);  // a为4,没悬念了吧？ 这里的a还是局部变量哦！
  var a;     // 局部变量a在这行声明
  alert(a);  // a还是为4,这是因为之前已把4赋给a了
}
test();
alert(a); // a为1，这里并不在function scope内，a的值为全局变量的值
```

PS：相对与window环境下的变量、函数声明，每一个作用域都会对其下的变量和函数进行先声明;

```js
function Hello() {
  alert("Hello");
}
Hello();
function Hello() {
  alert("Hello World");
}
Hello();
```

我们会看到这样的结果：连续输出了两次Hello World。而非我们想象中的Hello和Hello World。这是因为Javascript并非完全的按顺序解释执行，而是在解释之前会对Javascript进行一次“预编译”，在预编译的过程中，会把定义式的函数优先执行，也会把所有var变量创建，默认值为undefined，以提高程序的执行效率。也就是说上面的一段代码其实被JS引擎预编译为这样的形式。

```js
function Hello() {
  alert("Hello");
}
function Hello() {
  alert("Hello World");
}
Hello();
Hello();
```

我们可以通过上面的代码很清晰地看到，其实函数也是数据，也是变量，我们也可以对“函数“进行赋值（重赋值）。

当JavaScript引擎解析脚本时，它会在预编译期对所有声明的变量和函数进行处理。
做如下处理：

- 在执行前会进行类似“预编译”的操作：首先会创建一个当前执行环境下的活动对象，并将那些用var申明的变量设置为活动对象的属性，但是此时这些变量的赋值都是**undefined**，**并将那些以function定义的函数也添加为活动对象的属性，而且它们的值正是函数的定义。**
- 在解释执行阶段，遇到变量需要解析时，会首先从当前执行环境的活动对象中查找，如果没有找到而且该执行环境的拥有者有prototype属性时则会从prototype链中查找，否则将会按照作用域链查找。**遇到var a = …这样的语句时会给相应的变量进行赋值（注意：变量的赋值是在解释执行阶段完成的，如果在这之前使用变量，它的值会是undefined）**所以，就会出现当JavaScript解释器执行下面脚本时不会报错：

```js
alert(a);    // 返回值undefined
var a =1;
alert(a);    // 返回值1
```

由于变量声明是在预编译期被处理的，所以在执行期间对于所有代码来说，都是可见的。但是，你也会看到，执行上面代码，提示的值是undefined，而不是1。这是因为，变量初始化过程发生在执行期，而不是预编译期。在执行期，JavaScript解释器是按着代码先后顺序进行解析的，如果在前面代码行中没有为变量赋值，则JavaScript解释器会使用默认值undefined。由于在第二行中为变量a赋值了，所以在第三行代码中会提示变量a的值为1，而不是undefined

- 从如下结果中我们知道先是连续两次输出Hello Wrold!，最后连续两次输出test，得出这样的结果是因为javascript并非是完全按照顺序执行的，而是在执行之前先进行一个预编译，预编译 时声明式函数被提取出来，优先执行，而且相同的函数会进行覆盖，再执行赋值式函数。

```js
test();                    //输出Hello World!
function test(){　　　　　　
  alert('hello');　　　　　//声明式函数
}
test();                    //输出Hello World!

var test=function(){　　　　//赋值式函数
  alert('test');
}
test();                    //输出test
function test(){　　　　　　//声明式函数
  alert('Hello World!');
}
test();                    //输出test
```

实际执行顺序：

```js
var test=undefined；
function test(){　　　　　　
  alert('hello');　　　　　//声明式函数
}
function test(){　　　　　　//声明式函数
  alert('Hello World!');
}
test();                    //输出Hello World!
test();                    //输出Hello World!

var test=function(){　　　　//赋值式函数
  alert('test');
}
test();                    //输出test
test();                    //输出test
```

- 下面代码显示显示hello，再显示hello world！，这是因为javascript中的给个代码块是相互独立的，当脚本遇到第一个script标签时，则javascript 解析器会等这个代码块加载完成后，先对它进行预编译，然后再执行之，然后javascript解析器准备解析下一个代码块，由于javascript是按 块执行的，所有一个javascript调用下一个块的函数或者变量时，会出现错误:

```html
<script type='text/javascript'>
  function test(){
    alert('hello');                //显示hello
  }
  test()
</script>
<script type='text/javascript'>
  function test(){
    alert('hello world!');        //显示hello world！
  }
  test()
</script>
```

- 虽然javascript是按块执行的，但不同的块却属于相同的全局作用域，不同的块的变量和函数式可以相互使用的，也就是某个块可以使用前面块的变量和函数，却不可以使用它之后的块的变量和函数。

```html
<script type='text/javascript'>
  alert(name);                    //显示undefined
  var name='Jude';
  function test(){
    alert('hello');
  }
  fun();                            //不能调用下一个块的函数
</script>
<script type='text/javascript'>
  alert(name);           //可以调用上一个块的变量，显示Jude
  test();                //可以调用上一个块的函数，显示hello
  function fun(){
    alert('fun');
  }
</script>
```

综上所述，javascript在执行时的步骤是：

1. 先读入第一段代码块
2. 对代码块进行语法分析，如果出现语法错误，直接执行第5步骤

3. 对var变量和function定义的函数进行“预编译处理”（赋值式函数是不会进行预编译处理的）

4. 执行代码块，有错则报错

5. 如果还有下一段代码块，则读入下一段代码块，重复步骤2

#### 执行阶段

JavaScript在完成预编译后开始执行阶段，执行阶段具体有变量的赋值，函数的执行，执行的时候会创建JavaScript的执行上下文。

#### 什么是执行上下文？

简而言之，执行上下文是评估和执行 JavaScript 代码的环境的抽象概念。每当 Javascript 代码在运行的时候，它都是在执行上下文中运行。

#### 执行上下文的类型

JavaScript 中有三种执行上下文类型。

- **全局执行上下文** — 这是默认或者说基础的上下文，任何不在函数内部的代码都在全局上下文中。它会执行两件事：创建一个全局的 window 对象（浏览器的情况下），并且设置 `this` 的值等于这个全局对象。一个程序中只会有一个全局执行上下文。
- **函数执行上下文** — 每当一个函数被调用时, 都会为该函数创建一个新的上下文。每个函数都有它自己的执行上下文，不过是在函数被调用时创建的。函数上下文可以有任意多个。每当一个新的执行上下文被创建，它会按定义的顺序（将在后文讨论）执行一系列步骤。
- **Eval 函数执行上下文** — 执行在 `eval` 函数内部的代码也会有它属于自己的执行上下文，但由于 JavaScript 开发者并不经常使用 `eval`，所以在这里我不会讨论它。

##### 执行栈

执行栈，也就是在其它编程语言中所说的“调用栈”，是一种拥有 LIFO（后进先出）数据结构的栈，被用来存储代码运行时创建的所有执行上下文。

当 JavaScript 引擎第一次遇到你的脚本时，它会创建一个全局的执行上下文并且压入当前执行栈。每当引擎遇到一个函数调用，它会为该函数创建一个新的执行上下文并压入栈的顶部。

引擎会执行那些执行上下文位于栈顶的函数。当该函数执行结束时，执行上下文从栈中弹出，控制流程到达当前栈中的下一个上下文。

让我们通过下面的代码示例来理解：

```js
let a = 'Hello World!';

function first() {
  console.log('Inside first function');
  second();
  console.log('Again inside first function');
}

function second() {
  console.log('Inside second function');
}

first();
console.log('Inside Global Execution Context');
```

![上述代码的执行上下文堆栈](/assets/上述代码的执行上下文堆栈.png)

<center>上述代码的执行上下文堆栈</center>

当上述代码在浏览器中加载时，Javascript 引擎会创建一个全局执行上下文并将其推送到当前执行堆栈。当遇到调用`first()`时，Javascript 引擎会为该函数创建一个新的执行上下文，并将其推送到当前执行堆栈的顶部。

当从`second()`函数内部调用`first()`函数时，Javascript 引擎会为该函数创建一个新的执行上下文，并将其推送到当前执行堆栈的顶部。当`second()`函数结束时，它的执行上下文从当前栈中弹出，控件到达它下面的执行上下文，也就是`first()`函数执行上下文。

当`first()`完成时，它的执行堆栈从堆栈中移除并且控制到达全局执行上下文。执行完所有代码后，JavaScript 引擎会从当前堆栈中删除全局执行上下文。

##### 全局执行上下文

JavaScript 引擎创建了执行上下文栈（Execution context stack，ECS）来管理执行上下文

为了模拟执行上下文栈的行为，让我们定义执行上下文栈是一个数组：

```js
ECStack = [];
```

试想当 JavaScript 开始要解释执行代码的时候，最先遇到的就是全局代码，所以初始化的时候首先就会向执行上下文栈压入一个全局执行上下文，我们用 globalContext 表示它，并且只有当整个应用程序结束的时候，ECStack 才会被清空，所以程序结束之前， ECStack 最底部永远有个 globalContext：

```js
ECStack = [ globalContext ];
```

##### 函数执行上下文

现在 JavaScript 遇到下面的这段代码了：

```js
function fun3() {
  console.log('fun3')
}

function fun2() {
  fun3();
}

function fun1() {
  fun2();
}

fun1();
```

当执行一个函数的时候，就会创建一个执行上下文，并且压入执行上下文栈，当函数执行完毕的时候，就会将函数的执行上下文从栈中弹出。知道了这样的工作原理，让我们来看看如何处理上面这段代码：

```js
// 伪代码

// fun1()
ECStack.push(<fun1> functionContext);

// fun1中竟然调用了fun2，还要创建fun2的执行上下文
ECStack.push(<fun2> functionContext);

// fun2还调用了fun3！
ECStack.push(<fun3> functionContext);

// fun3执行完毕
ECStack.pop();

// fun2执行完毕
ECStack.pop();

// fun1执行完毕
ECStack.pop();

// javascript接着执行下面的代码，但是ECStack底层永远有个globalContext
```

##### Eval 函数执行上下文

```js
eval('var x = 10');

(function foo() {
  eval('var y = 20');
})();

alert(x); // 10
alert(y); // "y" 提示没有声明
ECStack的变化过程：

ECStack = [globalContext];

// eval('var x = 10');
ECStack.push({
  evalContext,
  callingContext: globalContext
});

// eval exited context
ECStack.pop();

// foo funciton call
ECStack.push(<foo> functionContext);

// eval('var y = 20');
ECStack.push({
  evalContext,
  callingContext: <foo> functionContext
});

// return from eval
ECStack.pop();

// return from foo
ECStack.pop();
```

#### 执行上下文是如何创建的？

到目前为止，我们已经了解了 JavaScript 引擎是如何管理执行上下文的，现在让我们了解一下 JavaScript 引擎是如何创建执行上下文的。

执行上下文在两个阶段中创建：**1）创建阶段**、**2）执行阶段。**

#### 创建阶段

在 JavaScript 代码执行前，执行上下文将经历创建阶段。在创建阶段会发生三件事：

1. **this** 值的决定，即我们所熟知的 **This 绑定**。
2. 创建**词法环境**组件。
3. 创建**变量环境**组件。

所以执行上下文在概念上表示如下：

```js
ExecutionContext = {
  ThisBinding = <this value>,
  LexicalEnvironment = { ... },
  VariableEnvironment = { ... },
}
```

##### This 绑定

在全局执行上下文中，`this` 的值指向全局对象。(在浏览器中，`this`引用 Window 对象)。

在函数执行上下文中，`this` 的值取决于该函数是如何被调用的。如果它被一个引用对象调用，那么 `this` 会被设置成那个对象，否则 `this` 的值被设置为全局对象或者 `undefined`（在严格模式下）。例如：

```js
let foo = {
  baz: function() {
  console.log(this);
  }
}

foo.baz();   // 'this' 引用 'foo', 因为 'baz' 被对象 'foo' 调用

let bar = foo.baz;
bar();       // 'this' 指向全局 window 对象，因为没有指定引用对象

```

##### 词法环境

[官方的 ES6](https://link.juejin.cn/?target=http%3A%2F%2Fecma-international.org%2Fecma-262%2F6.0%2F) 文档把词法环境定义为

> **词法环境**是一种规范类型，基于 ECMAScript 代码的词法嵌套结构来定义**标识符**和具体变量和函数的关联。一个词法环境由环境记录器和一个可能的引用**外部**词法环境的空值组成。

简单来说**词法环境**是一种持有**标识符—变量映射**的结构。（这里的**标识符**指的是变量/函数的名字，而**变量**是对实际对象[包含函数类型对象]或原始数据的引用）。

现在，在词法环境的**内部**有两个组件：(1) **环境记录器**和 (2) 一个**外部环境的引用**。

1. **环境记录器**是存储变量和函数声明的实际位置。
2. **外部环境的引用**意味着它可以访问其父级词法环境（作用域）。

**词法环境**有两种类型：

- **全局环境**（在全局执行上下文中）是没有外部环境引用的词法环境。全局环境的外部环境引用是 **null**。它拥有内建的 Object/Array/等、在环境记录器内的原型函数（关联全局对象，比如 window 对象）还有任何用户定义的全局变量，并且 `this`的值指向全局对象。
- 在**函数环境**中，函数内部用户定义的变量存储在**环境记录器**中。并且引用的外部环境可能是全局环境，或者任何包含此内部函数的外部函数。

**环境记录器**也有两种类型（如上！）：

1. **声明式环境记录器**存储变量、函数和参数。
2. **对象环境记录器**用来定义出现在**全局上下文**中的变量和函数的关系。

简而言之，

- 在**全局环境**中，环境记录器是对象环境记录器。
- 在**函数环境**中，环境记录器是声明式环境记录器。

**注意 —** 对于**函数环境**，**声明式环境记录器**还包含了一个传递给函数的 `arguments` 对象（此对象存储索引和参数的映射）和传递给函数的参数的 **length**。

抽象地讲，词法环境在伪代码中看起来像这样：

```js
GlobalExectionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Object",
      // 在这里绑定标识符
    }
    outer: <null>
  }
}

FunctionExectionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // 在这里绑定标识符
    }
    outer: <Global or outer function environment reference>
  }
}
```

##### 变量环境

它同样是一个词法环境，其环境记录器持有**变量声明语句**在执行上下文中创建的绑定关系。

如上所述，变量环境也是一个词法环境，所以它有着上面定义的词法环境的所有属性。

在 ES6 中，**词法环境**组件和**变量环境**的一个不同就是前者被用来存储函数声明和变量（`let` 和 `const`）绑定，而后者只用来存储 `var` 变量绑定。

我们看点样例代码来理解上面的概念：

```js
let a = 20;
const b = 30;
var c;

function multiply(e, f) {
 var g = 20;
 return e * f * g;
}

c = multiply(20, 30);
```

执行上下文看起来像这样：

```js
GlobalExectionContext = {

  ThisBinding: <Global Object>,

  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Object",
      // 在这里绑定标识符
      a: < uninitialized >,
      b: < uninitialized >,
      multiply: < func >
    }
    outer: <null>
  },

  VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Object",
      // 在这里绑定标识符
      c: undefined,
    }
    outer: <null>
  }
}

FunctionExectionContext = {
  ThisBinding: <Global Object>,

  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // 在这里绑定标识符
      Arguments: {0: 20, 1: 30, length: 2},
    },
    outer: <GlobalLexicalEnvironment>
  },

VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // 在这里绑定标识符
      g: undefined
    },
    outer: <GlobalLexicalEnvironment>
  }
}
```

**注意**：只有遇到调用函数 `multiply` 时，函数执行上下文才会被创建。

可能你已经注意到 `let` 和 `const` 定义的变量并没有关联任何值，但 `var` 定义的变量被设成了 `undefined`。

这是因为在创建阶段时，引擎检查代码找出变量和函数声明，虽然函数声明完全存储在环境中，但是变量最初设置为 `undefined`（`var` 情况下），或者未初始化（`let` 和 `const` 情况下）。

这就是为什么你可以在声明之前访问 `var` 定义的变量（虽然是 `undefined`），但是在声明之前访问 `let` 和 `const` 的变量会得到一个引用错误。这就是我们说的变量声明提升。

**注意**： 在执行阶段，如果 JavaScript 引擎不能在源码中声明的实际位置找到 `let` 变量的值，它会被赋值为 `undefined`。

**结论**：我们已经讨论过 JavaScript 程序内部是如何执行的。虽然要成为一名卓越的 JavaScript 开发者并不需要学会全部这些概念，但是如果对上面概念能有不错的理解将有助于你更轻松，更深入地理解其他概念，如变量声明提升，作用域和闭包。

> 本文参考文章连接 ：
>
> 1、 [Understanding Execution Context and Execution Stack in Javascript](https://blog.bitsrc.io/understanding-execution-context-and-execution-stack-in-javascript-1c9ea8642dd0)
>
> 2、[javascript运行过程中的“预编译阶段”和“执行阶段”](https://blog.csdn.net/weixin_36586564/article/details/77092067)


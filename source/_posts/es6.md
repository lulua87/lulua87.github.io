---
title: "ES6十大实用特性"
date: 2018-04-28 11:00:00
tags: ['ES6']
description: "ES6/ECMAScript2015 是一种新的javascript规范。"

---

## 首先回顾一下JavaScript的历史

下面就是一个简单的JavaScript发展时间轴：
1、1995：JavaScript诞生，它的初始名叫LiveScript。
2、1997：ECMAScript标准确立。
3、1999：ES3出现，与此同时IE5风靡一时。
4、2000–2005： XMLHttpRequest又名AJAX， 在Outlook Web Access (2000)、Oddpost (2002)，Gmail (2004)和Google Maps (2005)大受重用。
5、2009： ES5出现，（就是我们大多数人现在使用的）例如foreach，Object.keys，Object.create和JSON标准。
6、2015：ES6/ECMAScript2015出现。

## 以下是ES6排名前十的最佳特性列表（排名不分先后）

+ Default Parameters（默认参数） in ES6
+ Template Literals （模板文本）in ES6
+ Multi-line Strings （多行字符串）in ES6
+ Destructuring Assignment （解构赋值）in ES6
+ Enhanced Object Literals （增强的对象文本）in ES6
+ Arrow Functions （箭头函数）in ES6
+ Promises in ES6
+ Block-Scoped Constructs Let and Const（块作用域构造Let and Const）
+ Classes（类） in ES6
+ Modules（模块） in ES6

### 1.Default Parameters（默认参数）

举个简单例子

```js
var link = function (height, color, url) {    var height = height || 50;
    var color = color || 'red';
    var url = url || 'http://azat.co';
    ...
}
```

> <span style="color: red; font-size: 16px;">VS</span>

```js
var link = function(height = 50, color = 'red', url = 'http://azat.co') {  ...
}
```

### 2.Template Literals（模板对象）

举个简单例子

```js
var name = 'Your name is ' + first + ' ' + last + '.';
```

> <span style="color: red; font-size: 16px;">VS</span>

```js
var name = `Your name is ${first} ${last}. `;
```

### 3.Multi-line Strings （多行字符串）

举个简单例子

```js
var roadPoem = 'Then took the other, as just as fair,'
    + 'And having perhaps the better claim,'
    + 'Because it was grassy and wanted wear,'
    + 'Though as for that the passing there,'
    + 'Had worn them really about the same.';
```

> <span style="color: red; font-size: 16px;">VS</span>

```js
var roadPoem = `Then took the other, as just as fair,
    And having perhaps the better claim,
    Because it was grassy and wanted wear,
    Though as for that the passing there,
    Had worn them really about the same.`;
```

### 4.Destructuring Assignment （解构赋值）

举个简单例子

```js
var data = $('body').data(), // data has properties house and mouse
   house = data.house,
   mouse = data.mouse;
```

以及在node.js中用ES5是这样：

```js
var jsonMiddleware = require('body-parser').jsonMiddleware ;
var body = req.body, // body has username and password
   username = body.username,
   password = body.password;
```

> <span style="color: red; font-size: 16px;">VS</span>

在ES6，我们可以使用这些语句代替上面的ES5代码：

```js
var { house, mouse} = $('body').data();
var {jsonMiddleware} = require('body-parser');
var {username, password} = req.body;
```

### 5.Enhanced Object Literals （增强的对象字面量）

+ 函数类属性的省略语法

```js
const obj = {
    //Before
    foo: function(){
        return 'foo';
    },

    //After
    bar(){
        return 'bar';
    }
}
```

+ 支持 proto 注入

> 开发者允许直接向一个对象字面量注入`__proto__`，使其直接成为指定类的一个实例，而无须另外创建一个类来实现继承。

```js
import {EventEmitter} from 'events'

const machine = {
    __proto__: new EventEmitter(),

    method(){ /* …*/ },
    // …
}

console.log(machine);  //EventEmitter {}
console.log(machine instanceof EventEmitter)  //true

machine.on('event', msg => console.log(`Received message: ${msg}`));
machine.emit('event', 'hello world');
// Received message: hello world

machine.method(/* …. */);
```

+ 可动态计算的属性名

> ES6 引入的新语法允许我们直接使用一个表达式来表达一个属性名用法：{ [statement]: value}

```js
const prefix = 'ES6';
const obj = {
    [prefix + 'enhancedObject']: 'foo'
}
```

+ 将属性名定义省略

```js
const foo = 123;
const bar = () => foo;

const obj = {
    foo,
    bar
}

console.log(obj);  //{ foo: 123, bar: [Function] }
```

### 6.Arrow Functions in（箭头函数）

### 7.Promises

Promises的概念是由CommonJS小组的成员在Promises/A规范中提出来的，有许多略微不同的promise 实现语法。Q，bluebird，deferred.js，vow, avow, jquery 一些可以列出名字的。也有人说我们不需要promises，仅仅使用异步，生成器，回调等就够了。但令人高兴的是，在ES6中有标准的Promise实现。

下面是一个简单的用setTimeout()实现的异步延迟加载函数:

```js
setTimeout(function(){
  console.log('Yay!');
}, 1000);
```

> <span style="color: red; font-size: 16px;">VS</span>

```js
var wait1000 =  new Promise((resolve, reject)=> {
  setTimeout(resolve, 1000);
}).then(()=> {
  console.log('Yay!');
});
```

### 8.Block-Scoped Constructs Let and Const

### 9.Classes （类）
用ES5写一个类:

javascript传统做法是当生成一个对象实例，需要定义构造函数，然后通过new的方式完成。

```JS
function StdInfo(){
    this.name = "job";
    this.age = 30;
}

StdInfo.prototype.getNames = function (){
    console.log("name："+this.name);
}
//得到一个学员信息对象
var p = new StdInfo()
```

javacript中只有对象，没有类。它是是基于原型的语言，原型对象是新对象的模板，它将自身的属性共享给新对象。这样的写法和传统面向对象语言差异很大，很容易让新手感到困惑。

> <span style="color: red; font-size: 16px;">VS</span>

用ES6呢？

```JS
//定义类
class StdInfo {
    constructor(){
       this.name = "job";            
       this.age = 30;      
    }
    //定义在类中的方法不需要添加function
    getNames(){
       console.log("name："+this.name);      
    }
}
//使用new的方式得到一个实例对象
var p = new StdInfo();
```

> 使用extends关键字实现类之间的继承。这比在ES5中使用继承要方便很多。

```js
//定义类父类
class Parent {
    constructor(name,age){
        this.name = name;
        this.age = age;
    }

    speakSometing(){
        console.log("I can speek chinese");
    }
}
//定义子类，继承父类
class Child extends Parent {
    coding(){
        console.log("coding javascript");
    }
}

var c = new Child();

//可以调用父类的方法
c.speakSometing(); // I can speek chinese
```

使用继承的方式，子类就拥有了父类的方法。
> 如果子类中有constructor构造函数，则必须使用调用super。
 
```js
//定义类父类
class Parent {
    constructor(name,age){
        this.name = name;
        this.age = age;
    }

    speakSometing(){
        console.log("I can speek chinese");
    }
}
//定义子类，继承父类
class Child extends Parent {
    constructor(name,age){
        //不调super()，则会报错  this is not defined

        //必须调用super
        super(name,age);
    }

    coding(){
        console.log("coding javascript");
    }
}

var c = new Child("job",30);

//可以调用父类的方法
c.speakSometing(); // I can speek chinese
```

> 子类必须在constructor方法中调用super方法，否则新建实例时会报错(this is not defined)。这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。如果不调用super方法，子类就得不到this对象。

### 10.Modules （模块）

众所周知，在ES6以前JavaScript并不支持本地的模块。人们想出了AMD，RequireJS，CommonJS以及其它解决方法。现在ES6中可以用模块`import` 和`export` 操作了。
在ES5中，你可以在` <script>`中直接写可以运行的代码（简称`IIFE`），或者一些库像AMD。然而在ES6中，你可以用`export`导入你的类。下面举个例子，在ES5中,module.js有port变量和getAccounts 方法:

```js
module.exports = {
  port: 3000,
  getAccounts: function() {
    ...
  }
}
```

在ES5中，main.js需要依赖require(‘module’) 导入module.js：

```js
var service = require('module.js');
console.log(service.port); // 3000
```

> <span style="color: red; font-size: 16px;">VS</span>

但在ES6中，我们将用export and import。例如，这是我们用ES6 写的module.js文件库：

```js
export var port = 3000;
export function getAccounts(url) {
  ...
}
```

如果用ES6来导入到文件main.js中，我们需用`import {name} from ‘my-module’`语法，例如：

```js
import {port, getAccounts} from 'module';
console.log(port); // 3000
```

或者我们可以在main.js中把整个模块导入, 并命名为 service：

```js
import * as service from 'module';
console.log(service.port); // 3000
```

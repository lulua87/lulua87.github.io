---
title: "CommonJS规范"
date: 2018-04-28 13:00:00
tags: ['CommonJS']
description: "模块化的好处： 为了更有效的组织代码，提高重用性，增大开发效率，我们会把项目拆分成不同模块，每个模块，职责单一，多人协同，高效运行，易于维护。"

---
> 模块化的好处： 为了更有效的组织代码，提高重用性，增大开发效率，我们会把项目拆分成不同模块，每个模块，职责单一，多人协同，高效运行，易于维护。

## 模块的发展历程

+ js 不像其他高级语言有模块系统，标准库较少，缺乏包管理系统。
+ js 起初只有全局对象的形式，通过一个个小函数来实现不同的模块功能。
+ 渐渐发展，通过构建对象的形式，来封装不同的功能。
+ 继续发展，通过立即执行函数和闭包的形式来分离一个又一个的小组件。
+ 当对象多起来的时候，又开始通过命名空间，来实现分级管理。
+ 最终，历经十多年，社区渐渐发展壮大，`commonJS规范`的提出成了javascript 历史上最重要的里程碑：Best Wishes : hope javascript can run everywhere!

## CommonJS规范

CommonJS只是一个规范，不是文件或者框架，这点一定要清楚。它的官方网址：http://www.commonjs.org/ ，CommonJS中大部分只是草案或者说是理论，在近几年的发展中已有显著成效，这些规范包括：

+ 模块
+ 二进制
+ I/O流
+ 进程环境
+ 文件系统
+ 套接字
+ 单元测试
+ web服务器网关
+ 包管理等等。

在CommonJs规范中：

+ 一个文件就是一个模块，拥有单独的作用域；
+ 普通方式定义的变量、函数、对象都属于该模块内；
+ 通过`require`来加载模块；
+ 通过`exports`和`modul.exports`来暴露模块中的内容；

1. 所有代码都运行在模块作用域，不会污染全局作用域；
2. 模块可以多次加载，但只会在第一次加载的时候运行一次，然后运行结果就被缓存了，以后再加载，就直接读取缓存结果；
3. 模块的加载顺序，按照代码的出现顺序是同步加载的;

> 在后端`nodejs`显然已经是`CommonJS规范的最佳实践`。

## nodejs中的require的内部机制

我们知道，在nodejs 中每个文件就是一个独立的模块。在模块之间不会造成变量污染和命名冲突等问题。通过`exports` 或者 `module.exports` 输出功能或者说提供API，我们通过`require` 来引入一个模块，很像前端通过`script`标签来引入js文件。但是它的内部细节又如何呢，在朴灵的那本《深入浅出》中有关javascript的模块编译那块已经做了一些讲解，那么我们现在去繁留简来深入体验一把：

require 函数的源码可以这样理解，当然我们简化了不少东西，现在我们只把它抽离出来 :

```js
// 模拟require的实现
function _require(path) {
    // 定义一个Module对象
    var Module = function() {
        this.exports = {};
    }

    // 引入nodejs 文件模块 下面是nodejs中原生的require方法
    var fs = require('fs');

    // 同步读取该文件
    var sourceCode = fs.readFileSync(path, 'utf8');

    // 头尾拼接包装成新的字符串
    var packSourceCode = '(function(module,exports){ ' + sourceCode + ' return module.exports; })';

    // 字符串转换成函数
    var packFunc = eval(packSourceCode);

    // 实例化一个Module 里面有一个exports属性
    var module = new Module();

    // 把module 和 它内部的module.exports都作为参数传进去 
    // 并得到挂在到module.exports 或 exports上的功能
    var res = packFunc(module, module.exports);

    // 最终我们拿到了path代表的文件模块提供的API
    return res;
}
```

> 说明： 上面举了一个小栗子来模拟nodejs中的require的实现，当然源码不会这样写，源码涉及的东西会更复杂些。上面只是一些帮助理解的小demo。

我们自己写的逻辑功能全在path所代表的文件模块中，在内部把所有功能挂在到module.exports 或者exports对象上，最终return 他们得以暴露。

我们可以看出 module.exports 和 exports 指向的是同一对象

在require一个模块时大致经历了这几步：

+ 路径分析
+ 文件定位
+ 编译执行

## 一个小的 require 模块引入 案例

在 util.js文件模块中，我们这样写

```js
// 定义一个计算器对象
var Calculator = function() {};
Calculator.prototype = {
    add: function(x, y) {
        return x + y;
    },
    subtract: function(x, y) {
        return x - y;
    },
    multiply: function(x, y) {
        return x * y;
    },
    divide: function(x, y) {
        return x / y;
    }
}
// 虽然exports 和 module.exports 指向同一个对象
// 但是此处不能使用 exports = new Calculator(); 具体原因，自行百度

module.exports = new Calculator(); // 暴露API对象
```

在 index.js 文件中，我们这样写：

```js
var cal = require('./util');
var res = cal.add(1, 2);
console.log(res); // 3
```

在 CLI 中，我们执行index模块，将会输出3
`$ node index`

## nodejs中的模块分类和加载顺序

nodejs 中核心模块和文件模块的引用方式：

+ 核心模块： require(‘核心模块名’);
+ 文件模块： require(‘路径+文件名’); 路径可以用./代表的相对路径或者绝对路径，其中文件名可以省略后缀名。
+ 自定义模块：特殊的文件模块，可能是一个文件或者包的形式

模块加载顺序 ：

1. 优先从缓存加载
2. 核心模块：如http、fs、path等 (优先查找核心模块)
3. 文件模块： ./或../开始的相对路径文件模块，以/开始的绝对路径文件
4. 自定义模块：查找最费时

目录和包查找原则:
比如有如下的模块路径： 查找规则是沿路径向上逐级递归，直到根目录的`node_modules`目录:
├─/node_modules/  
└─/home/node_modules/
└─/user/test/node_modules/

这就是自定义模块加载速度最慢的原因了。

1. 当我们require 的标识符 不包含扩展名, node会按照`.js`,`.json` ,`.node` 的次序补足扩展名 ，依次尝试。

2. 如果在require过程中，没有查找到对应文件，却得到一个目录，此时 node 会将当前目录当作一个包来处理。

3. 此时，node 会查找目录下的`package.json`文件，通过`JSON.parse()` 解析包描述对象，从中拿到main属性指定的文件名进行定位。

4. 如果`main`属性指定错误，或者没有package.json文件，那么node会将index作为默认的文件名，去依次查找index.js , index.json , index.node。

5. 在目录分析中没有定位到任何模块，那么它会遍历自己的上一级目录进行查找，如果还没找到，抛出查找失败的异常。

> ps：`npm init -y`：初始化一个package.json文件，加上-y就会默认生成该文件，无需一步一步填写
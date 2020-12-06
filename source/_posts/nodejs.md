---
title: "nodejs基础入门"
date: 2017-09-04 11:00:00
tags: ['node', 'node开发']
description: "作为前端开发工程师，需要学会利用nodejs进行开发。"

---

### nodejs
Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境。
Node.js 使用了一个事件驱动、非阻塞式 I/O 的模型，使其轻量又高效。
Node.js 的包管理器 npm，是全球最大的开源库生态系统。

### 名称解释
#### V8引擎
V8是Google Chrome浏览器的JavaScript引擎，通过对高性能V8引擎的封装，并通过一系列优化的API类库，使其就能够在后端运行了。

#### 事件驱动，非阻塞式
[Dan York介绍了两种典型的事件驱动实例](http://code.danyork.com/2011/01/25/node-js-doctors-offices-and-fast-food-restaurants-understanding-event-driven-programming/)

> 第一个例子是关于医生看病。

在美国去看医生，需要填写大量表格，比如保险、个人信息之类，传统的基于线程的系统（thread-based system），接待员叫到你，你需要在前台填写完成这些表格，你站着填单，而接待员坐着看你填单。你让接待员没办法接待下一个客户，除非完成你的业务。

想让这个系统能运行的快一些，只有多加几个接待员，人力成本需要增加不少。

基于事件的系统（event-based system）中，当你到窗口发现需要填写一些额外的表格而不仅仅是挂个号，接待员把表格和笔给你，告诉你可以找个座位填写，填完了以后再回去找他。你回去坐着填表，而接待员开始接待下一个客户。你没有阻塞接待员的服务。

你填完表格，返回队伍中，等接待员接待完现在的客户，你把表格递给他。如果有什么问题或者需要填写额外的表格，他给你一份新的，然后重复这个过程。

这个系统已经非常高效了，几乎大部分医生都是这么做的。如果等待的人太多，可以加入额外的接待员进行服务，但是肯定要比基于线程模式的少得多。

> 第二个例子是快餐店点餐。

在基于线程的方式中（thread-based way）你到了柜台前，把你的点餐单给收银员或者给收银员直接点餐，然后等在那直到你要的食物准备好给你。收银员不能接待下一个人，除非你拿到食物离开。想接待更多的客户，容易！加更多的收银员！

当然，我们知道快餐店其实不是这样工作的。他们其实就是基于事件驱动方式，这样收银员更高效。只要你把点餐单给收银员，某个人已经开始准备你的食物，而同时收银员在进行收款，当你付完钱，你就站在一边而收银员已经开始接待下一个客户。在一些餐馆，甚至会给你一个号码，如果你的食物准备好了，就呼叫你的号码让你去柜台取。关键的一点是，你没有阻塞下一个客户的订餐请求。你订餐的食物做好的事件会导致某个人做某个动作（某个服务员喊你的订单号码，你听到你的号码被喊到去取食物），在编程领域，我们称这个为`回调`（callback function）。

> Node.Js做了什么工作呢？

传统的web server多为`基于线程模型`。你启动Apache或者什么server，它开始等待接受连接。当收到一个连接，server保持连接连通直到页面或者什么事务请求完成。如果他需要花几微妙时间去读取磁盘或者访问数据库，web server就阻塞了IO操作（这也被称之为阻塞式IO).想提高这样的web server的性能就只有启动更多的server实例。

相反的，Node.Js使用`事件驱动模型`，当web server接收到请求，就把它关闭然后进行处理，然后去服务下一个web请求。当这个请求完成，它被放回处理队列，当到达队列开头，这个结果被返回给用户。这个模型非常高效可扩展性非常强，因为webserver一直接受请求而不等待任何读写操作。（这也被称之为非阻塞式IO或者事件驱动IO）

考虑下面这个过程：

1，你用浏览器访问nodejs服务器上的"/about.html"

2，nodejs服务器接收到你的请求，调用一个函数从磁盘上读取这个文件。

3，这段时间，nodejs webserver在服务后续的web请求。

4，当文件读取完毕，有一个回调函数被插入到nodejs的服务队列中。

5，nodejs webserver运行这个函数，实际上就是渲染（render）了about.html页面返回给你的浏览器。

好像就节省了几微秒时间，但是这很重要！特别是对于需要相应大量用户的web server。

#### 包管理器 npm
NPM是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题，常见的使用场景有以下几种：

允许用户从NPM服务器下载别人编写的第三方包到本地使用。
允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。


### 重要概念
#### IO
磁盘文件系统或者数据库的写入和读出。

#### process对象
process对象保存了一些当前进程的一些信息：

+ process.pid：进程id
+ process.versions：node\v8和其他组建的版本号
+ process.arch：系统架构
+ process.argv：命令行接口参数
+ process.env：环境变量
+ process.uptime()：获取uptime
+ process.memoryUsage()：获取占用内存
+ process.cwd()：获取当前工作目录
+ process.exit()：结束当前进程
vprocess.on()：添加监听事件

#### stream (流) 流式读写
流（stream）在 Node.js 中是处理流数据的`抽象接口`（abstract interface）。
Node.js 提供了多种流对象。 例如， `HTTP` 请求 和 `process.stdout` 就都是`流的实例`。

stream 模块可以通过以下方式引入：

```js
const stream = require('stream');
```

所有的流都是 EventEmitter 的实例。
常用的事件有：

data - 当有数据可读时触发。
end - 没有更多的数据可读时触发。
error - 在接收和写入过程中发生错误时触发。
finish - 所有数据已被写入到底层系统时触发。

流可以是可读的、可写的，或是可读写的。

Node.js 中有四种基本的流类型：

Readable - 可读的流 (例如 fs.createReadStream()).
Writable - 可写的流 (例如 fs.createWriteStream()).
Duplex - 可读写的流 (例如 net.Socket).
Transform - 在读写过程中可以修改和变换数据的 Duplex 流 (例如 zlib.createDeflate()).

流式读写隐藏在node的各个地方：

HTTP requests and responses
Standard input/output
File reads and writes


#### 管道(pipe)
管道机制一开始是UNIX中出现的，一个程序的输出直接成为下一个程序的输入，就像水流过管道一样方便。
管道提供了一个输出流到输入流的机制。通常我们用于从一个流中获取数据并将数据传递到另外一个流中。

```js
var fs = require("fs");
// 创建一个可读流
var readerStream = fs.createReadStream('input.txt');
// 创建一个可写流
var writerStream = fs.createWriteStream('output.txt');
// 管道读写操作
// 读取 input.txt 文件内容，并将内容写入到 output.txt 文件中
readerStream.pipe(writerStream);
console.log("程序执行完毕");
```


#### Buffer， 缓冲器
在 ECMAScript 2015 (ES6) 引入 `TypedArray` 之前，JavaScript 语言没有读取或操作二进制数据流的机制。 Buffer 类被引入作为 Node.js API 的一部分，使其可以在 TCP 流或文件系统操作等场景中处理二进制数据流。

JavaScript里的String对象，存储的是字符串，而且是Unicode编码的。
Buffer代表一个缓冲区，存储二进制数据，是字节流。我们在网络传输时，就传输的这种字节流。写文件时，也是写的字节流。

#### child_process(子进程)
通过child_process模块创建新进程，从而实现多核并行计算。
虽然，Nodejs天生是单线程单进程的，但是有了child_process模块，可以在程序中直接创建子进程，并使用主进程和子进程之间实现通信，等到子进程运行结束以后，主进程再用回调函数读取子进程的运行结果。

#### cluster (集群)
每个worker进程通过使用`child_process.fork()`函数，基于IPC（Inter-Process Communication，进程间通信），实现与master进程间通信。

#### 事件循环
`Event loop`有大量的异步操作完成时需要调用相应回调函数，需要一种机制来管理执行先后，这种机制就叫做事件循环。为一个回调函数队列，node.js不断查询队列中是否有事件，查询到事件，调用相应javascript函数，机制为先进先出任务队列。

### 参考
[you-dont-know-node](https://webapplog.com/you-dont-know-node/)
[nodejs的一些核心概念](https://blog.csdn.net/bdss58/article/details/51429105)


---
title: "Koa"
date: 2018-05-09 17:50:00
tags: ['Koa', 'web 框架']
description: "Koa 是一个新的 web 框架，由 Express 幕后的原班人马打造， 致力于成为 web 应用和 API 开发领域中的一个更小、更富有表现力、更健壮的基石。 通过利用 async 函数，Koa 帮你丢弃回调函数，并有力地增强错误处理。 Koa 并没有捆绑任何中间件， 而是提供了一套优雅的方法，帮助您快速而愉快地编写服务端应用程序。"

---
### [Koa](https://koa.bootcss.com/) 是什么？

### 应用场景
+ 用作本地服务器

```js
const Koa = require('koa');
const app = new Koa();

app.use(async ctx => {
  ctx.body = 'Hello World';
});

app.listen(3000)
```


+ 其他
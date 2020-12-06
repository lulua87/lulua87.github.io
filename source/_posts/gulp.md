---
title: "Gulp"
date: 2018-04-05 10:50:00
tags: ['gulp', '前端本地开发']
description: "gulp 是基于 node 实现 Web 前端自动化开发的工具，利用它能够极大的提高开发效率。 在 Web 前端开发工作中有很多“重复工作”，比如压缩CSS/JS文件、css/js预编译，postcss等方案，浏览器前缀自动补全，监听文件变化、自动刷新页面等等。。。"

---
### [Gulp](https://www.gulpjs.com.cn/) 是什么？

`用自动化构建工具增强你的工作流程！`

gulp是一个基于流的构建工具，可以自动执行指定的任务，简洁且高效

### Gulp 能做什么？

开发环境下，想要能够按模块组织代码，监听实时变化

+ css/js预编译，postcss等方案，浏览器前缀自动补全等
+ 条件输出不同的网页，比如app页面和mobile页面
+ 线上环境下，我想要合并、压缩 html/css/javascritp/图片，减少网络请求，同时降低网络负担
+ 等等...

例子：对src目录下的js进行压缩，并输出到dist目录下

```js
const gulp = require('gulp');

gulp.task('default', function() {
    gulp.src('js/*.js')
        .pipe(uglify())
        .pipe(gulp.dest('dist'));
});
```

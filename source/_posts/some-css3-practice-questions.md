---
title: "一些实用的CSS3问题及解决"
date: 2016-05-09 20:24:00
tags: ['css3', '技巧']
description: "text-overflow主要功能是用来截取文本长度，用（...）来代替截掉的文本，原本这个效果以前主要靠js和程序来完成，那么现在我们可以直接使用CSS3来实现"


---

### text-overflow实现文本(...)截取省略效果

语法：

  text-overflow ： clip | ellipsis
 

取值说明：

1、clip:表示不显示省略标记(...)，而只是简单的裁切，需要在一定的高度范围内配合overflow:hidden属性使用，如果不配合的话将无任何效果；

2、ellipsis：对象文本溢出时将显示省略标记(...)，需要配合overflow:hidden；white-space:nowrap一起使用才会有效果。
HTML如下:
```js
<div class="demo text-overflow-ellipsis">用我来测试text-overflow:ellipsis的性能和使用方法。</div>
```
对应的CSS:

```js
.text-overflow-ellipsis {
    -o-text-overflow: ellipsis;
    text-overflow: ellipsis;    
    overflow: hidden;
    white-space: nowrap; //让文本不换行
  }
```
text-overflow:ellipsis到目前firefox4.0都还不支持这个属性.

解决思路: 
一、Firefox的伪类──:after

HTML如下:
```js
<div class="demo text-overflow">
     <span>>est AH,test AH,test AH,test AH,test AH,test AH,test AH,test AH,test AH,test AH,test AH,test AH,test AH</span>
  </div>
```

CSS如下:
```js
 .text-overflow {
    overflow: hidden; /*清除浮动*/
 }
 /*Safari,Opera,IE下效果*/
 .text-overflow span{
   display: block;
   width: 100px; /*设置内容宽度*/
   overflow: hidden;/*隐藏溢出的文本*/
   white-space: nowrap;/*让文本不换行*/
   -o-text-overflow: ellipsis;/*Opera下实现ellipsis效果*/
   text-overflow: ellipsis;/*Safari，IE下实现ellipsis效果*/
 }
        
 /*Firefox实现效果*/
 @-moz-document url-prefix(){ /*@-moz-document url-perfix(){}是firefox的一个独有属性，只有firefox浏览器能识别，也可以说是一种hack*/
   .text-overflow span {
     max-width: 70px;/* 在FF下改变内容宽度，用来放置:after增加的内容(...)*/
     float: left;/*进行浮动*/
   }
 }
 @-moz-document url-prefix(){ 
   /*利用:after增加(...)省略符*/
   .text-overflow:after {
      content:"...";/*增加省略符号*/
      float: left;/*设置浮动*/
      width: 25px;/*省略符宽度*/
      padding-left: 5px;/*省略符内距，用来拉开内容之间的距离*/
      color: #000;
   }
 }
 ```



### 使用::Selection
选择文本，背景是红色，而文本是白色的问题，
![::Selection](http://cdn.w3cplus.com/cdn/farfuture/uIe9qU7h16JBryuv8OGphj7O1YZp2aJqKuCODjRoLak/mtime:1341237777/sites/default/files/selection-red.png)

::selection使用语法：

```js
/*Webkit,Opera9.5+,Ie9+*/
::selection {
    background: #F00;
    color: #FFF;
}
/*Mozilla Firefox*/
::-moz-selection {
    background: #F00;
    color: #FFF;
}
```


### Box-sizing

[box-sizing是CSS3的box属性之一](http://www.w3cplus.com/content/css3-box-sizing)。一说到CSS的盒模型（Box model）我想很多人都会比较烦.

CSS中Box model是分为两种，第一种是W3C的标准模型，另一种是IE的传统模型，他们相同之处都是对元素计算尺寸的模型，具体说就是对元素的width,height,padding,border以及元素实际尺寸的计算关系；他们不同之处呢？两者的计算方法不一至.

语法：

  box-sizing ： content-box || border-box || inherit

取值说明

1. content-box:此值为其默认值，其让元素维持W3C的标准Box Model，也就是说元素的宽度/高度（width/height）等于元素边框宽度（border）加上元素内边距（padding）加上元素内容宽度/高度（content width/height）即：Element Width/Height = border+padding+content width/height。

2. border-box:此值让元素维持IE传统的Box Model（IE6以下版本），也就是说元素的宽度/高度等于元素内容的宽度/高度。（从上面Box Model介绍可知，我们这里的content width/height包含了元素的border,padding,内容的width/height【此处的内容宽度/高度=width/height-border-padding】）。


### 字符相关设置

#### word-wrap

语法：

   word-wrap ： normal | break-word
 

取值说明：

1. normal和break-word，其中normal为默认值，当其值为normal控制连续文本换行(允许内容顶开容器的边界，换句话说内容可以撑破容器）；

2. break-word将内容在边界内换行（不截断英文单词换行，截断英文单词换行需要使用word-break:all属性）.


#### word-break使用：

上面我们使用word-wrap:break-word只能在内容中换行，而不能实现词内换行，前面提到过如果需要词内换行，我们需要使用word-break属性，下面我们就一起来看看这个属性：

语法：

   word-break:normal | break-all | keep-all
 

取值说明

1. normal为默认值，如果设置为默认值时中文则到边界处的汉字换行，如果是英文整个单词换行，如果出现某个单词长度过长，则会撑破容器，如果边框为固定属性，则后面部分将无法显示；

2. break-all：可以强行截断英文单词，达到词内换行效果.

3. keep-all:不允许字断开。如果是中文将把前后标点符号内的一个汉字短语整个换行，英文单词也整个换行，如果出现某个英文字符长度超过边界，则后面的部分将撑破容器，如果边框为固定属性，则后面部分无法显示.


#### white-space属性

作用
  处理空白符.

语法:

  white-space: normal || pre || nowrap || pre-line || pre-wrap || inherit


#### 字间距word-spacing  和 字符间距letter-spacing



### 清楚浮动-- clearfix
只需要添加这个类名无需加上任何HTML标记，就可以达到清除浮动的效果：

```js

.clearfix:before,
.clearfix:after {
  content: " ";
  display:table;
}
 
.clearfix:after {
  clear:both;
  overflow:hidden;
}

.clearfix {
  zoom: 1;
}

```
### 使用!important
使用!important可以覆盖任何相同的样式，换句话说他可以改为样式的权重：

```js
p {
  font-size:20px !important;
}
```
但是,不建议过多使用, 会造成维护困难的问题.

### 垂直居中, 水平居中, 垂直水平居中

#### 水平居中
让一个元素<strong>水平居中</strong>对于CSS来说非常简单：如果是一个内联元素，我们可以在他的父元素上设置text-align:center;；如果是一个块元素，我们可以使用margin:auto;。然而，只要一想到让一个元素<strong>垂直居中</strong>，让人死的心都有了。

#### 垂直居中

解决方案有:

1. 表格布局不讨论(表格显示模式)，因为它需要一些冗余的HTML标签
2. inline-block方法不包括，因为要使用很多Hack手段
(1) 宽度高度均已知的父元素和子元素: 
  ![示例图(1)](https://raw.githubusercontent.com/lulua87/MarkdownPhotos/master/20160509/position-centered.gif)
  [DEMO](https://css-tricks.com/320-quick-css-trick-how-to-center-an-object-exactly-in-the-center/)
(2) 
  ![示例图(2)](https://raw.githubusercontent.com/lulua87/MarkdownPhotos/master/20160509/unknown-position.png)

解决方法:
1. 最简单粗暴的方法是: 使用table布局
```js
<table style="width: 100%;">
  <tr>
     <td style="text-align: center; vertical-align: middle;">
          Unknown stuff to be centered.
     </td>
  </tr>
</table>
```

如果你担心语义化的问题,可以使用如下方式:

```js
<div class="something-semantic">
   <div class="something-else-semantic">
       Unknown stuff to be centered.
   </div>
</div>
```

然后CSS这样:

```js
.something-semantic {
  display: table;
  width: 100%;
}
.something-else-semantic {
  display: table-cell;
  text-align: center;
  vertical-align: middle;
}
```
![ghost方式](https://raw.githubusercontent.com/lulua87/MarkdownPhotos/master/20160509/ghost.png)
```js
/* This parent can be any width and height */
.block {
  text-align: center;

  /* May want to do this if there is risk the container may be narrower than the element inside */
  white-space: nowrap;
}
 
/* The ghost, nudged to maintain perfect centering */
.block:before {
  content: '';
  display: inline-block;
  height: 100%;
  vertical-align: middle;
  margin-right: -0.25em; /* Adjusts for spacing */
}

/* The element to be centered, can also be of any width and height */ 
.centered {
  display: inline-block;
  vertical-align: middle;
  width: 300px;
}
```
### TODO
3. 绝对定位解决方案
4. 视窗单位的解决方案
5. Flexbox的解决方案
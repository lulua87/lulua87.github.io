---
title: "前端如何实现视觉设计稿"
date: 2017-08-29 19:00:00
tags: ['H5', '视觉设计稿']
description: "作为前端开发工程师，往往需要视觉和UI设计师给出设计图，包括psd稿和视觉标注图，前端才能进行开发。"


---

在这篇文章中将和大家探讨一下关于前端在移动端开发如何去实现视觉设计稿。探讨过后，在大家的实际工作中或许能帮助解决一些问题。


### 前端工程师需要明白的「像素」

一般设计稿是640px或者*750px*（现在最流行），但是iPhone 5不是320px宽吗，iPhone 6不是375px宽吗?
这里需要理解一下基础概念： *设备像素*(device pixel)，*CSS像素*(css pixel)以及*设备像素比*(device pixel ratio)。

+ 设备像素(device pixel):
设备像素设是物理概念，指的是设备中使用的物理像素。
比如iPhone 5的分辨率`640 x 1136px`。

+ CSS像素(css pixel):
CSS像素是Web编程的概念，指的是CSS样式代码中使用的逻辑像素。
在CSS规范中，长度单位可以分为两类，绝对(absolute)单位以及相对(relative)单位。px是一个相对单位，相对的是设备像素(device pixel)。

+ 设备像素比(device pixel ratio):
即`window.devicePixelRatio`，是设备上物理像素和设备独立像素(device-independent pixels (dips))的比例。
公式表示就是`window.devicePixelRatio = 物理像素 / dips`

垂直手机屏幕下，使用`&lt;meta name="viewport" content="width=device-width"&gt;`，iphone5屏幕物理像素640像素，独立像素还是320像素，因此，`window.devicePixelRatio`等于2。

![Image text](https://raw.githubusercontent.com/lulua87/image/master/2017-08-29/pixel.png)

比如iPhone 5，6使用的是Retina视网膜屏幕（2倍屏），6plus是3倍屏，使用2px x 2px的`device pixel` 代表`1px x 1px `的 `css pixel`，所以设备像素数为`640 x 1136px`（5），`750 x 1134`（6），而CSS逻辑像素数为`320 x 568px`（5），`375 x 667`（6）；5，6的`window.devicePixelRatio=2`，6plus 为3。

![Image text](https://raw.githubusercontent.com/lulua87/image/master/2017-08-29/iphone.png)

### H5适配：rem方案
rem：是CSS3新增的一个相对单位，相对于`html标签`的`font-size`的大小为基础的。而`font-size`的大小可以动态根据*手机屏幕宽度*`document.documentElement.clientWidth`来设置，从而达到*自适应屏幕*的目的。

我这里找了一下[小米](https://m.mi.com/)，[网易](http://3g.163.com/touch/)，[拉勾网](https://m.lagou.com/)，[手淘](https://m.taobao.com) 以及糯米，大同小异。


#### [小米官网](https://m.mi.com/) 
![Image text](https://raw.githubusercontent.com/lulua87/image/master/2017-08-29/mi.jpg)

设计稿是720px的，即5英寸屏幕的安卓手机（720 x 1280px）。
对于页面缩放和横竖屏事件进行监听，改变html根元素字体`clientWidth/720/100`。
如图是这样计算的`375/(720/100) = 52.0833`

#### [网易](http://3g.163.com/touch/) 
![Image text](https://raw.githubusercontent.com/lulua87/image/master/2017-08-29/163.png)

![Image text](https://raw.githubusercontent.com/lulua87/image/master/2017-08-29/163-js.png)

iphone6 : `375/7.5=50`, 则知道设计稿应该是基于iphone6来的，所以它的设计稿竖直放时的横向分辨率为750px，为了计算方便，取一个100px的font-size为参照，那么body元素的宽度就可以设置为`width: 7.5rem`，于是html的`font-size=deviceWidth / 7.5`。布局时，设计图标注的尺寸除以100得到css中的尺寸。并且布局中的`font-size`也可用`rem`单位。


#### [拉勾网](https://m.lagou.com/) 
![Image text](https://raw.githubusercontent.com/lulua87/image/master/2017-08-29/lagou6.png)
![Image text](https://raw.githubusercontent.com/lulua87/image/master/2017-08-29/lagouIpad.jpg)

```js
html {
	font-size: 65.5%;
}

```
设置html根元素字体为`65.5%`，对应px单位则为`10.48px`，则列表里时间信息字体设置为`1rem = 10.48px`，chrome在`-webkit-text-size-adjust: 100%;`情况下小于`12px`的一律显示为`12px`。

拉勾网页面列表部分是`px`为单位，字体是`rem`，底部bar是使用`百分百`来控制宽高间距。


之前网上讨论的比较多的是
```js
body {
	font-size: 62.5%;
}
p {
 	font-size: 1.2em;
}
```
则`1em = 16px * 62.5% = 10px`，em的初始值为`1em = 16px`，而为了方便计算， 换算一下`10 / 16`（16px是chrome浏览器默认字体大小）。缺点是进行任何元素设置，都有可能需要知道他父元素的大小，比较繁琐低效。




#### [手淘](https://m.taobao.com) 
![Image text](https://raw.githubusercontent.com/lulua87/image/master/2017-08-29/tao.jpg)

（1）动态设置viewport的scale

```js
var scale = 1 / devicePixelRatio;
document.querySelector('meta[name="viewport"]').setAttribute('content','initial-scale=' + scale + ', maximum-scale=' + scale + ', minimum-scale=' + scale + ', user-scalable=no');
```

（2）动态计算html的font-size

```js
document.documentElement.style.fontSize = document.documentElement.clientWidth / 10 + 'px';
```

（3）布局的时候，各元素的css尺寸=设计稿标注尺寸/设计稿横向分辨率/10

设计稿是750的，所以html的font-size就是75，如果某个元素是150px的宽，换算成rem就是`150 / 75 = 2rem`。


整个手淘设计师和前端开发的适配协作基本思路是：

+ 选择一种尺寸作为设计和开发基准
+ 定义一套适配规则，自动适配剩下的两种尺寸(介于iphone6的小屏和大屏)
+ 特殊适配效果给出设计效果

![Image text](https://raw.githubusercontent.com/lulua87/image/master/2017-08-29/tao-design-parten.png)

手淘推出了一套移动端适配的方案——[flexible方案](https://github.com/amfe/lib-flexible)。

总结来说：
+ 动态读取设备宽度并结合设备的像素比 
+ 动态改变html的font-size大小 & 页面缩放比例 
+ 影响以rem为单位的元素的最终呈现

#### [糯米](https://bj.nuomi.com/)

+ 糯米某活动H5
![Image text](https://raw.githubusercontent.com/lulua87/image/master/2017-08-29/nuo.png)

设计稿是`640`的，html字体设置为屏幕宽度，以iphone6为例，`font-size: 375px`; 如果某个元素是`150px`的宽，换算成`rem`就是`150/640rem`。

---

### px方案：css尺寸为对应设计稿／2
设计稿是750的。
![Image text](https://raw.githubusercontent.com/lulua87/image/master/2017-08-29/px-6.png)
![Image text](https://raw.githubusercontent.com/lulua87/image/master/2017-08-29/px-5.png)

优点：简单粗暴，所有css尺寸均为设计稿尺寸直接除2，开发快速简单；
缺点：可能出现一排放不下的情况，需要针对小屏幕如5及以下做单独适配

### vw方案
+ [糯米WAP](https://m.nuomi.com)
利用CSS3中新增单位<mark>vw</mark>，配合<mark>百分比</mark>来做响应式开发。

| 单位 | 释义 | 说明 | 
| - | :-: | -: | 
| px | 相对于显示器屏幕分辨率| - | 
| em | 相对于父元素字体大小 | - | 
| rem | 相对于根元素字体大小 | css3 |
| vw | 相对于视窗的宽度 | css3 |
| vh | 相对于视窗的高度 | css3 |

`vw` 相对于视窗的宽度：视窗宽度是`100vw`。
 如果视区宽度是100vm, 则1vm是视区宽度的1/100, 也就是1%，类似于width: 1%。
 那iphone6来说，`document.documentElement.clientWidth=375`, 则豆腐块宽度为 `375/100*30=112.5px`

![Image text](https://raw.githubusercontent.com/lulua87/image/master/2017-08-29/nuomi-app.jpg)
![Image text](https://raw.githubusercontent.com/lulua87/image/master/2017-08-29/nuomi-app-c.jpg)

---

### 混合: rem px vw 百分百等单位混用
+ rem & 百分比%
```js
body {
	padding-bottom: 14.0625%;
}

a.link {
	width: 30vw;
	height: 23vw;
}

```
略，同上糯米WAP

+ rem & vw
```js
html {  
    font-size: 4.375vw;
}

```
这里假设设计稿640px，则设置根元素font-size为4.375vw，根据屏幕宽度自适应，在视窗宽度为 320px 的时候，正好是 14px (14 / 320 = 0.04375)。 达到页面默认字体大小14px的目的（其他大小也ok）。好了，现在页面上所有以 `rem` 为单位的属性值都会随着屏幕的宽度变化而变化，达到自适应的目的。（`自适应不用js动态设置根元素大小`）

```js
p {  
    font-size: 1rem;   /* 设计稿上为 14px */
    padding-top: 2rem: /* 设计稿上为 28px */
}
```

### 总结
在移动端页面开发中，视觉童鞋一般会用750px（iphone 6）来出设计稿，然后要求FE童鞋能够做到页面是自适应屏幕的，这种情况下就可以用rem或者vm等相对单位来做适配，愉快和视觉童鞋一起玩耍啦。




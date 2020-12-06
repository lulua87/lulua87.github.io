---
title: "前端开发模式总结"
date: 2019-03-07 15:50:00
tags: ['框架', '构建打包', '前端开发模式']
description: "前端开发涉及面很广，最近想做一个关于前端开发模式迭代的总结分享，由于近些年前端开发的框架和构建打包工具层出不穷，让人眼花缭乱，而这些框架或者工具的最终目的是为了提升开发效率、提升代码可维护性。前面先会简单过一下前端大而全的一些东西，后面会逐渐讲一些开发中实用的前端开发模式和框架，顺便回顾一下近些年来开发过的项目，最后做一下总结。"

---

## 一、前言

前端开发涉及面很广，最近想做一个关于前端开发模式迭代的总结分享，由于近些年前端开发的框架和构建打包工具层出不穷，让人眼花缭乱，而这些框架或者工具的最终目的是为了提升开发效率、提升代码可维护性。前面先会简单过一下前端大而全的一些东西，后面会逐渐讲一些开发中实用的前端开发模式和框架，顺便回顾一下近些年来开发过的项目，最后做一下总结。

## 二、前端三要素：HTML、CSS、JS

1、HTML可以看做是一个房子的骨架，结构。
2、如果说HTML是房子的骨架，那么CSS就是房子的外观。
3、那么一些负责交互、功能性质的工作就得交给Javascript了！其中，js由三部分组成：ECMAScript（ES，语法、标准规范） + DOM（文档对象模型）+ BOM（浏览器对象模型）。

### HTML之于模板引擎

除了静态网站，大部分网站数据来源于后端，通过某种方式输出到页面，这里后面会讲到。由此，模板的诞生是为了将显示与数据分离，例如后端模板[smarty](https://www.smarty.net/)、前端JS模板[BaiduTemplate](http://baidufe.github.io/BaiduTemplate/)，模板技术多种多样，但其本质是将模板文件和数据通过模板引擎生成最终的HTML代码。
<img src="/download/attachments/690531984/tpl.jpg?version=1&modificationDate=1550563548667&api=v2"></img>

### JS、CSS之于ES6/Es7/TypeScript、Less/Sass/Stylus

原生JS、CSS被浏览器原生支持，但是这些原生语言有他们的弊端：
1、JS有这些被诟病的“痛点”：弱类型和没有命名空间，导致很难模块化，不适合开发大型应用；
2、CSS不支持模块化、变量、Mixin(混入)等实用功能，随着网站复杂度越来越高，同时考虑项目维护成本，使用更高级的语言势在必行。
因此为了提升开发效率，我们一般使用更方便开发、更好维护、支持模块化的语言，JS->ECMAScript 6（ES6、ES7,下一代标准）、TypeScript（JS的超集）, CSS->CSS预处理语言(Less、Sass/Scss、Stylus等)。


如下图，通过编译工具(例如[Webpack](https://www.webpackjs.com/))，这些高级语言最终编译为浏览器可直接运行的为JS、CSS。
<img src=""></img>
<p style="margin-left:350px;">借助编译工具</p>

## 三、应用形态

一、从移动端和PC端来区分，主要分为：
### 1、移动应用

<img src=""></img>

#### 1. Web App

Web App即移动端的网站，将页面部署在服务器上，然后用户使用各大浏览器访问。一般泛指 SPA(Single Page Application)模式开发出的网站。

#### 2. Native App

Native App即传统的原生APP开发模式，Android基于Java语言，底层调用Google的 API；iOS基于OC或者Swift语言，底层调用App官方提供的API。不在本次讨论范围内。

#### 3. Hybrid App，介于Web App和Native App

这里简单讲一下近些年流行的一种开发模式——Hybrid App。众所周知，原生APP开发中有一个`webview`的组件(Android中是webview，iOS7以下有UIWebview、7以上有WKWebview)，这个组件可以加载Html文件。反过来，Html页面利用`JSBridge`，可以调用Native的api，达到与原生APP基本一致的用户体验，例如拍照存储，本地相册上传等操作。

<img src=""></img>

如上图，这种Hybrid混合开发模式，由Native通过JSBridge等方法提供统一的API，然后用Html5+JS来写实际的逻辑，可去调用Native API。在这种模式下，由于Android、iOS的API对外暴露出的一致性、且最终的页面在webview中显示，故此达到`跨平台`的目的。

### 2、PC Web 应用

这里忽略。

二、从页面跳转形式来看，主要分为：

### 1.单页面应用（SinglePage Web Application，SPA）

只有一张Web页面的应用，单页面跳转仅刷新局部资源 ，公共资源(js、css等)仅需加载一次，常用于PC端官网、购物等网站。
如下图:
<img src=""></img>
<p><span style="display:inline-block;margin-left:230px;">单页面应用结构视图</span></p>

### 2.多页面应用（MultiPage Application，MPA）

多页面跳转刷新所有资源，每个公共资源(js、css等)需选择性重新加载。
如下图:
<img src=""></img>
<p><span style="display:inline-block;margin-left:230px;">多页面应用结构视图</span></p>

## 四、前端开发模式的迭代

前端开发给人的印象一直是变化太快，新的框架、库、构建工具、开发模式层出不穷。
<img src=""></img>

### 一、传统开发模式

Application Server 负责提供动态内容，浏览器发起请求后，由 Application Server 返回 `HTML 文档` 或 `JSON 数据`，浏览器解析到 HTML 内的 script、link、img 等资源标签时，会发起资源文件的请求，静态资源 URL 通常会加上专门的域名解析到静态资源 Server，对于小型 Web 系统，Application Server 和静态资源 Server 会由同一个 Web Server 来承载。

<img src=""></img>

<span style="color:red">动态HTML</span>衍生出了两种传统的前端开发模式：
1、HTML 混合在 Server 端程序代码中：
这种模式下的协作方式通常是由前端工程师开发好静态页面，再交给后端工程师『套页面』，最后产出一个包含 Server 端程序代码和 HTML 的代码文件，浏览器请求时由 Server 端程序执行代码文件，获取数据、拼接 HTML 片段生成完整的 HTML 文档。缺点很明显就是前后端未分离，协作效率极低。
2、通过 Server 端模板引擎生成 HTML：
模板引擎的出现为前后端提供了更好的协作方式，它是一个运行在 Server 端应用程序中的组件，能清晰的将前端代码与 Server 端程序代码分离。

以基于 PHP 的 Smarty 模板引擎为例，如下图

<img src="">smarty模板demo</span></p>

调用 assign 方法向模板中传递变量/对象，调用 display 方法来显示模板，生成 HTML。
assign 和 display 确定了前后端的衔接方式：约定一份模板变量、一个模板文件路径。

### 二、前后端分离

Application Server 提供的动态内容除了 HTML，还可以是<span style="color:red">JSON</span>数据，这又衍生出另一种开发模式：前后端分离。

前后端分离带来了巨大的好处：
> 1.后端工程师只负责提供 API 接口，可以专注于 API 的实现。
> 2.一套 API 可以提供给浏览器、IOS、Android 等多端使用。
> 3.前后端可以完全分开部署，后端不用再承担模板渲染、页面路由配置等 UI 层的工作。
> 4.前端可以把控用户交互、页面路由、内容渲染的整个过程，在开发效率、功能实现方案、用户体验上会有更大的探索空间。

前后端分离有两种模式：
1、基于 Node 的前后端分离
2、单页 Web 应用（SPA， Single-page application）

#### 基于 Node 的前后端分离

基于 Node 的前后端分离依然会有 Server 端渲染页面的逻辑，只是这个 Server 端是由前端工程师通过 Node 搭建的，整体流程如下：
<img src=""></img>

浏览器发起的请求由 Node Server 来处理，Node Server 接收到请求后访问相应的 API Server 获取数据，再生成 HTML 文档返回给浏览器。
开发 Server 端应用就需要有一个 Server 端的应用框架，基于 Node 的应用框架有很多，包括：Express、Koa、阿里的 Egg、百度的 Yog2 等等。这个第五章的第5小节会讲到。

#### 单页 Web 应用(SinglePage Web Application，SPA)

单页 Web 应用通常没有 Server 端渲染模板的逻辑，前端产出的都是静态资源，整体流程如下：
<img src=""></img>

浏览器访问应用的各个 URL 时 Web Server 都会 rewrite 到一个静态 HTML：

```js
location / {
    root /home/www/app;
    index index.html;
    // !-f用来判断是否存在文件 ；$request_filename:当前请求的文件路径，由root或alias指令与URI请求生成
    if (!-f $request_filename) {
        // 通配符正则，匹配到任何路径，均重写到/index.html
        rewrite ^(.*)$ /index.html break;
    }
}
```

但是一般情况下，前端在本地开发，也可以有多种方式来启动一个Web Server服务，例如利用Node.js配合[http-server](https://www.npmjs.com/package/http-server)、或者[Glup配合BrowserSync](https://www.gulpjs.com.cn/docs/recipes/server-with-livereload-and-css-injection/)可以快速启动一个本地服务。

这个 HTML 通常没有内容，只是一个加载 JS 的页面：

```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <title>vue-demo</title>
  <link rel="stylesheet" href="/index.md5.css" />
</head>

<body ontouchstart>
  <div id="app"></div>
  <!-- built files will be auto injected -->
  <script src="/index.md5.js"><script/>
</body>

</html>
```

单页 Web 应用与普通网站的重要区别是：普通网站的入口是 HTML，而单页 Web 应用的入口实则是 JS，由 JS 解析路由、从 API Server 取数据、渲染内容。

具体到应用的开发，依然需要有开发规范与框架的支撑，主流的技术栈包括：Angular、React、Vue。这个第五章会讲到。

## 五、不同类型项目回顾

随着业务的不同，每段时期前端团队的分化点如下：
1、重型SPA页面，重业务逻辑复杂，iframe集成（糯米营销mis）。
2、Hybrid方式的Web页面，糯米APP，发布升级免审核，注重组件化开发、跨平台性、贴近原生。
3、活动页面，例如糯米3.7女生节、5.7吃货节，主要活动形式摇一摇、秒杀（糯米节日大促）。
4、H5小游戏，例如红包雨、捉螃蟹、堆蛋糕，优惠券、积分、红包等激励（糯米拉新留存）。
5、移动端H5内容检索类，例如招聘、房产、结婚等新垂类（大搜导流），主要兼顾SEO、首屏渲染速度。
6、小程序([百度智能小程序](https://smartprogram.baidu.com/docs/develop/tutorial/demo/))

下面，我按照时间纬度讲述一下上面列的这些不同分化方向的具体项目：

### 一、重型SPA的MIS

我是从15年开始入职前端开发的，那一年印象最深的是我配合后端同学开发了10+个内网使用的后台管理系统，这些Mis系统大多是前端框架[Angular1.0](https://www.angular.cn/)和百度出品的前端工程构建工具[FIS-PLUS](https://github.com/fex-team/fis-plus/wiki)配合使用，其中数据图表使用百度出品的[Echarts](https://echarts.baidu.com/)，UI框架使用[Bootstrap](https://getbootstrap.com/)，且多个Mis系统之间使用iframe方案集成到一个叫[营销门户](http://www.mkt.nuomi.com/oam#/index)的主营销Mis系统，该主Mis包括了菜单修改配置、人员权限配置、审批流等公用功能。

FIS-PLUS 是基于 FIS，应用于后端是 PHP，模板是 Smarty 的场景。一般，FISP目录规范如下：

```js
site
|____plugin   // Smarty插件相关，解析*.tpl
|       |____compiler.body.php
|       |____compiler.head.php
|       |____compiler.html.php
|       |____compiler.require.php
|       |____compiler.script.php
|       |____compiler.style.php
|       |____compiler.widget.php
|       |____......
|____static // 静态资源
|       |____css
|       |     |____bootstrap // UI框架
|       |     |       |____fonts
|       |     |       |____less
|       |____img 
|       |____js
|       |     |____angular.js 
|       |     |____echarts.js 
|       |     |____mod.js 
|       |     |____ui-bootstrap-tpls.js // bootstrap框架下的UI控件
|       |____receiver.php // 用于推送前端代码到后端开发机联调
|____module1
|       |____page
               |____index.tpl // 入口模板，需后端编译，且依赖Smarty插件解析
        |____widget
               |____header
                      |____header.tpl
                      |____header.js
                      |____header.less
               |____banner
                      |____banner.tpl
                      |____banner.js
                      |____banner.less 
        |____fis.conf // fis构建打包配置

```

<img src=""></img>

其中重要一点，FIS提供了定位资源能力，可以有效地分离开发路径与部署路径之间的关系：
<img src=""></img>

index.tpl示例如下：

```html
{%html framework="oam:static/js/mod.js"%}
{%head%}
    {%block name="meta"%}
        <meta charset="utf-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
        <meta name="viewport" content="width=device-width" />
        <link rel="shortcut icon" href="http://s3.bae.baidu.com/hao123storage/data/oam_16_16_272cfa04e4014981ce736a784176466a"/>
    {%/block%}
    <title>{%block name="title"%}营销门户{%/block%}</title>
    {%require name="oam:static/css/bootstrap/less/bootstrap.less"%}
    {%require name="oam:static/css/common.less"%}
    {%require name="oam:static/js/angular/angular.js"%}
    {%require name="oam:static/js/angular-ui-router.js"%}
    {%require name="oam:static/js/angular/angular-growl.js"%}
    {%require name="oam:static/js/moment.js"%}
    {%require name="oam:static/js/ui-bootstrap-tpls-0.12.0.js"%}
    {%require name="oam:static/js/echarts-all.js"%}
{%/head%}
{%body%}

<div growl></div>
<page-header></page-header>
<div class="container">

    <div class="col-md-2 col-sm-2">
        <sidebar>
        </sidebar>
    </div> 
    <div class="col-md-10 col-sm-10 pageBody">
        <iframe height='3000px' width='1058px' ng-show='iframeUrl' ng-src="{{iframeUrl|trustAsResourceUrl}}" frameborder="0" id="framePage" scrolling="auto"></iframe>
        <div ng-hide='iframeUrl'>
            <div ui-view></div>
        </div>
    </div>
</div>

{%block name="body"%}
    {%script%}
        require.async(["oam:widget/init/init.js"],function(){
            angular.element(document).ready(function() {
            angular.module('oamApp', ['oamMod']);
                angular.bootstrap(document, ['oamApp']);
            });
        });
    {%/script%}
{%/block%}
{%/body%}
{%/html%}
```

重型的SPA页面，业务功能复杂，使用Vue，React，Angular这种MVVM的框架，在开发过程中，很多懒加载和延迟加载之类的也是不需要做。因为相关的数据后面都需要用到，也就不存在是否会使用的问题。

### 二、Hybrid做糯米APP

16年至17年接触[糯米APP](https://m.nuomi.com/)的组件开发，采用[Hybrid](https://www.cnblogs.com/yexiaochai/p/4921635.html) 解决方案[百度Hybrid开放平台](http://developer.nuomi.com/#/home)达到跨平台支持、贴近原生的用户体验，这里大部分糯米组件(例如附近页、积分商城、我的)主要是利用前端框架[Vue.js](https://cn.vuejs.org/v2/guide/)其天然支持组件化的特性来支持前端同学组件化开发，Ajax异步接口调用，达到多个前端内部高度分治，前后端分离的目的；

简单来说，组件包是基于业务开发的一系列组件化页面的集合,是用于组织某个业务功能所涉及的H5页面的单位。组件包是由至少1个H5页面和一个config.json配置文件的页面集合压缩而成。
百度组件化技术是一种Hybrid开发技术，主要有以下特点：

1. 用基于Webview的H5页面取代原生页面
2. 为组件提供一套统一的调用Native能力的API,消除平台差异 — BNJS
3. 以组件为单位管理h5页面
组件包id（示例名为demo）唯一，需在百度Hybrid开放平台申请注册。组件包内每个页面需包含一个配置文件config.json，配置名称和是否登录，图标icon以及接口是否预加载等：

<img src=""></img>
<p style="margin-left:400px;">Hybrid开放平台</p>

一份完整的配置文件config.json范例如下：

```json
{
  "id": "demo",
  "version": "1.0.3",
  "pages": [
    {
      "name": "home",
      "file": "/page/index.html",
      "login": false,
      "prehttp":[
        {
          "url":"http://app.nuomi.com/naserver/search/searchitem",
          "action":"postNA",
          "params":{
              "page_idx":0,
              "goods_per_page":15,
              "locate_city_id":"${location.cityCode}",
              "page_type":"component",
              "logpage":"SearchList",
              "sidlist":"${env.sidList}",
              "sort_type":0,
              "keywords":"${keyword}",
              "s_brother":"",
              "backupList":""
          }
        }
      ],
    },
    {
      "name": "user",
      "file": "/page/user.html",
      "login": true
    }
  ],
  "tpl": "nuomi",
  "domainlist": ["app.nuomi.com"],
  "webp_proxy": {
      "enable": false,
      "include": [],
      "exclude": []
  },
  "resources":[
      "__BNJSComPackage/js/zepto/1.1.6/zepto.min.js",
      "__BNJSComPackage/cs/animate/3.5.0/animate.min.css",
      "__BNJSComPackage/img/loading_1/1.0.0/loading_1.png"
  ],
  "canPreload": 1,  
  "https": [
      "op.juhe.cn",
      "app.nuomi.com"
  ],
  "public_resource": {
      "enable": true,
      "enable_comps": [
          "index"
      ]
  },
  "default_ua": 1
}
```

组件包的生命周期如下图：
<img src="">Hybrid开放平台</p>

而H5页面一般可以用vue、react等框架基础上开发：

```js
import Vue from 'vue';

// 首页
import VueComponentApp from './index.vue';
import Pages from '../config/page.js';

BNJSReady(() => {
    BNJS.ui.title.setTitle('我的积分商城');
    BNJS.ui.title.addActionButton({
        tag: '23',
        text: '积分解读',
        callback() {
            BNJS.statistic.addLog({
                actionID: 'Points_Rules',
                actionExt: '积分_积分规则点击量'
            });
            BNJS.page.start(Pages.points_rules);
        }
    });

    new Vue({
        el: '#app',
        render(h) {
            return h(VueComponentApp);
        }
    });
    BNJS.ui.hideLoadingPage();
});
```

### 三、H5输出平台

17年由于各个产品线运营同学对产品营销推广的庞大需求，例如H5页面促销活动（3.7女生节、5.7吃货节，摇一摇，秒杀）、落地推广、商业广告变现等需求，急需推出一个针对糯米全网甚至全厂的、满足运营同学能随时随地配置并输出活动页面的平台，[活动运营配置平台](http://omg.int.nuomi.com)在这种情况下应运而生，极大地促进了前端同学的人力成本降为10%，截至目前该平台已输出活动5000+，该平台采用多种技术框架(Vue.js开发平台组件、React.js开发平台操作性功能、各种中间件)揉和在一起，主要<span style="color:red">技术难点</span>在于平台组件(促销组件：例如刮刮乐、老虎机、九宫格、弹幕、优惠券等，功能组件：例如轮播图、表单、TAB切换、城市定位等)可拖拽拼合成页面、独立组件关联、多团单页面性能、页面实时预览、人员权限分配管理等。

<img src=""></img>
<p style="margin-left:400px;">活动配置平台-可拖拽配置Demo</p>

用户在平台上通过配置多个组件拼装活动内容会生成对应js和css文件的cdn地址(`cdnJsUrl`、`cdnCssUrl`)，并和当前的活动id(`ac_id`)绑定。

用户配置完活动可以先预览效果，预览URL示例：`http://****/cplatform/wap/mkt?is_preview=1&cdnJsUrl=https://nmhdbos.nuomi.com/1548-1551932374-181466643-cdnJsUrl.js&cdnCssUrl=https://nmhdbos.nuomi.com/1548-1551932374-181466643-cdnCssUrl.css&ac_id=1548`
<img src=""></img>
<p style="margin-left:100px;">活动配置平台-大体框架图</p>

1.NA上mkt-framework组件（baidu/tuan-mkt-bc/mkt-framework）

这个是活动在na上运行的框架，它只是个壳的作用，内容的渲染是：会根据url的参数活动id（ac_id）去请求后端接口，拿到页面配置的元素生成的js、css文件的cdn地址，加载并运行，另外一种是参数上直接带cdn地址（如上的预览示例URL），如下index.js。

这里采用上一节讲到的配置文件config.json，采用预加载方式提高页面加载速度：

```json
{
    "name":   "framework_index",
    "login":  false,
    "prehttp":[{
        "url":"https://huodong.nuomi.com/acplat/data/acbasic",
        "action":"post",
        "params":{
            "ac_id":"${ac_id}"
        }
    }]
  }
```

这个壳index.html如下：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <title>百度糯米</title>
    <meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no">
    <meta name="format-detection" content="telephone=no">
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"/>
</head>
<body compid="noComp">
    <section id="page-loading">
        <span class="waite-loading"></span>&nbsp;加载中...
    </section>
    <div id="app-wrapper">
        <section id="app"></section>
    </div>
</body>
</html>
```

index.js主要逻辑截取如下:

```js
BNJSReady(function () {
    BNJS.ui.hideLoadingPage();
    BNJS.page.getData(function (res) {
        var params = res;

        //如果是单组件na或wap预览
        if(params.cdnJsUrl && params.cdnCssUrl){
            BNJS.ui.title.setTitle('单组件预览');
            MKT.loadCss(params.cdnCssUrl,function(){

            },function(){
                BNJS.ui.showTips('页面样式加载失败', 0);
            })
            .loadJs(params.cdnJsUrl, function () {
                document.getElementById('app-wrapper').style.visibility = 'visible';
                var $loading = document.getElementById('page-loading');
                $loading && ($loading.style.display = 'none');
            },function(){
                BNJS.ui.showTips('页面脚本加载失败', 0);
            });
            return;
        }

        MKT.acid = params.ac_id || '';
        MKT.tsmcid = params.tsmcid || `mkt_${params.ac_id}`;

        //发起统计
        handleBaiduidAndSendPv(`mkt_${params.ac_id}`);

        // 如果是wap环境则在发布后静态文件是后端渲染到页面上
        // 在预览环境则是url上直接传js和css的cdn地址
        if (BNJS.env.appName === "bainuo-wap") {
            // 设置异常情况下页面展现
            switch (+MKT_CONF.status) {
                case 2:
                    BNJS.ui.showTips('活动已下线', 0);
                    break;
                case 3:
                    BNJS.ui.showTips('活动未开始', 0);
                    break;
                case 4:
                    BNJS.ui.showTips('活动结束', 0);
                    break;
            }

            document.getElementById('app-wrapper').style.visibility = 'visible';
            return;
        }

        // 糯米NA上的处理逻辑
        //请求参数
        var reqParam = {};
        params.ac_id && (reqParam.ac_id = params.ac_id);
        params.is_preview && (reqParam.is_preview = params.is_preview);
        BNJS.http.post({
            url: config.getActConfig,
            params: reqParam,
            onSuccess: function (res) {
                if (res.errno == 0) {
                    var data = res.data;
                    // 设置活动标题
                    BNJS.ui.title.setTitle(data.ac_title);
                    // 加载组件列表
                    if (params.is_preview == 1 || data.status == 1) {
                        var staticInfo;
                        try{
                            staticInfo = JSON.parse(data.static_info);
                        }catch(e){
                            BNJS.ui.showTips('页面解析错误', 0);
                        }

                        if (staticInfo && staticInfo.cdnCssUrl && staticInfo.cdnJsUrl) {
                             MKT.loadCss(staticInfo.cdnCssUrl,function(){
                             },function(){
                                BNJS.ui.showTips('页面样式加载失败', 0);
                             })
                             .loadJs(staticInfo.cdnJsUrl, function () {
                                document.getElementById('app-wrapper').style.visibility = 'visible';
                                document.getElementById('page-loading').style.display = 'none';
                                console.log('获取脚本距页面初始化时间：');
                                console.log(Date.now()-startTime);
                            },function(){
                                BNJS.ui.showTips('页面脚本加载失败', 0);
                            });
                        }else{
                            BNJS.ui.showTips('页面内容加载失败', 0);
                        }
                    }
                    else {
                        // ...
                    }
                }
            }
        });
    });
});
```

2.wap的实现：
wap的基本原理和NA上是一致的，通过把组件的html、js、css文件放到对应的后端环境下，分为平台预览环境和线上环境。这种首先的问题是解决bnjs接口跨域的问题。

3.BNJS改造：
因为BNJS一些数据的初始化是通过请求m.nuomi.com实现的，如account、location等。如果平台上直接使用的话会存在跨域问题，所以解决办法就有： 
1）运营这边rd支持这些接口 
2）让m.nuomi.com支持从运营平台这边（huodong.nuomi.com）的跨域请求 
3）前端重新开发一套BNJS，避免去请求那些会跨域的接口 
最后基于可靠、功能、时间成本选择了方案2

4.预览环境
分为前端：baidu/tuan-mkt-bc/custom-mkt-web，后端（Node.js+KOA.js，也是fe同学负责）：baidu/tuan-mkt-bc/custom-mkt-server，最后接口API和数据库表由运营rd同学负责。

将组件的index.html、index.js、base.js、index.css和BNJS分别放到后端代码对应的位置，然后添加个路由（wap/preview）渲染这个模板即可。
待解决的问题是预览环境的域不是huodong.nuomi.com，m.nuomi.com还不支持跨域，可以先通过挂代理的方式去做

5.线上环境
线上模块是baidu/tuan-fe-bj-b/sas-acttpl（FIS-Plus支持），通过把组件的静态文件放到对应的目录里，让rd配置访问的路由。因为这个项目模板是由smarty渲染，所以上面1提到的壳index.html需要修改成smarty模板（index.tpl），同时可以把内容区域渲染的cdn文件地址等数据让rd动态渲染到页面里面。

index.tpl示例如下：

```tpl
<!DOCTYPE html>
{%html%}
    {%head%}
        <meta charset="utf-8"/>
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="initial-scale=1.0, maximum-scale=1.0, user-scalable=no, width=device-width" />
        {%block name="seo"%}
            <meta name="keywords" content="{%$data.keywords|default:'百度糯米'%}" />
            <title>{%$data.ac_title|default:'百度糯米'%}</title>
        {%/block%}
        <style type="text/css">
            body {
                opacity:0
            }
            .BNJS-body-with-loading {
                position: relative;
                width: 100%;
                height: 100%;
                background-color: #EEE
            }
        </style>
        <link rel="stylesheet" type="text/css" href="{%$data.static_info.cdnCssUrl%}">
        <link rel="stylesheet" type="text/css" href="/static/css/mkt-index.css">        
        <script>     	
            var MKT_CONF = {
                title: '{%$data.ac_title|escape:javascript%}', 
                shareImg: '{%$data.share_descpic_url|escape:javascript%}',
                shareDesc: '{%$data.share_desc|escape:javascript%}',
                shareTitle: '{%$data.share_title|escape:javascript%}' || '{%$data.ac_title|escape:javascript%}',
                desc: '{%$data.ac_desc|escape:javascript%}',
                actId: '{%$data.id|escape:javascript%}',
                status: '{%$data.status|escape:javascript%}'
            }

            {%*手百分享配置*%}
            var BoxShareData = {
                "successcallback": "bdboxSuccess",
                "errorcallback": "bdboxError",
                "options": {
                    "type": "url",
                    "mediaType": "all",
                    "linkUrl": location.href,
                    "title": '{%$data.share_title|escape:javascript%}' || '{%$data.ac_title|escape:javascript%}',
                    "content": '{%$data.share_desc|escape:javascript%}',
                    "iconUrl": '{%$data.share_descpic_url|escape:javascript%}'
                }
            };


            {%*bnjs运行依赖的数据，正常是由后端注入到页面中，暂时写死*%}
            window.pageInfo = {
                "account":{
                    "uid":null,
                    "uname":null,
                    "displayname":null,
                    "encrypt_phone":null,
                    "bduss":"BDUSS",
                    "pass_portrait":"",
                    "nuomi_portrait":"",
                    "nuomi_nickname":""
                },
                "city":{
                    "cityCode":"",
                    "cityName":"",
                    "shortCityName":"",
                    "cityUrl":""
                },
                "component":{
                    "id":"mkt-component",
                    "version":"1.0.5",
                    "pages":[
                        {"name":"framework_index","file":"/mkt-framework/index/index.html","login":false}
                    ],
                    "https":["se.nuomi.com"],
                    "currentPage":"mkt-framework/index/index.html",
                    "sid": ""
                },
                "site":{
                    "prefix":"https://m.nuomi.com/component/mkt-framework/1.0.5",
                    "config":{
                        "city-select":false,
                        "title":false,
                        "force-redirect":true,
                        "home-button":false,
                        "no-share-icon":false,
                        "on-open-fail":"redirect",
                        "theme":"wap",
                        "home-button-url":"http://m.nuomi.com",
                        "hijack-link-click":"false",
                        "broadcast-notice-in-weixin":"false",
                        "redirect-map":"",
                        "getData":[]
                    },
                    "currentPage":"framework_index"
                },
                "customUi":[],
                "env":{
                    "payHost":"https://zhifu.baidu.com",
                    "requestUrl":"//m.nuomi.com/webapp/bnjs/request",
                    "naserverHost":"//app.nuomi.com",
                    "loginHost":"https://wappass.baidu.com",
                    "userInfoBaiduHost":"//nuomiwappassport.baidu.com",
                    "rootUrl":"https://devers.baidu.com/cdn/static/comp_to_wap/carsales/2.0.6",
                    "network":"1"
                },
                "schema":{
                    "tuandetail":"http://m.nuomi.com/webapp/tuan/detail?deal_id=${tuanid}",
                    "search":"http://m.nuomi.com/webapp/tuan/search",
                    "merchantmap":"http://m.nuomi.com/webapp/tuan/shopmap?merchantId=${shopid}${seller_id}",
                    "categorylist":"http://m.nuomi.com/${category}/0-0/0-0-0-0-0-0/0-0",
                    "searchresult":"http://m.nuomi.com/webapp/tuan/list?${keyword}",
                    "t10comp":"//m.nuomi.com/component?compid=${compid}&comppage=${comppage}",
                    "panorama":"//m.nuomi.com/component?compid=wap-widget&comppage=panorama&uid=${uid}",
                    "commentlist":"http://m.nuomi.com/webapp/tuan/comment?deal_id=${tuanid}",
                    "home":"//m.nuomi.com",
                    "searchresultcomp":"//m.nuomi.com/component?compid=${compid}&comppage=${comppage}&keyword=${keyword}"
                },
                "getData":""
            };
        </script>
        {%block name="head"%}{%/block%}
    {%/head%}

    {%body%}
        {%block name="content"%}
            <div id=app-wrapper>
                <section id=app></section>
            </div>
            <section id="page-error">
                <p class="error-text"></p>
                <p class="error-refresh-bnt"></p>
            </section>

        {%/block%}	

        {%if $data.id == 324 || $data.id == 451 %}
            <script src="/static/js/BNJS.without.location.js"></script>
        {%else%}
            <script src="/static/js/BNJS.min.js"></script>
        {%/if%}

        <script src="/static/js/mkt-base.js"></script>
        <script src="/static/js/mkt-index.js"></script>
        <script src="/static/js/aio.js"></script>
        <script src="{%$data.static_info.cdnJsUrl%}"></script>
        <script src="//res.wx.qq.com/open/js/jweixin-1.0.0.js"></script>		
    {%/body%}
{%/html%}
```

### 四、Node做服务端渲染，前后端同构

18年初，我们组高工采用[Vue-SSR](https://ssr.vuejs.org/zh/guide/)的SSR(服务端渲染)技术方案，搭建了一套基于Vue.js + Node.js + Koa.js(服务框架) + Webpack(前端构建)的SSR框架。这套框架适用于招聘、房产、结婚等新垂类，使用的好处是既能用我们前端组同学熟悉的技术栈、快速开发上线，又能兼顾SEO、首屏渲染速度快，但是最大缺点是对机器部署有要求(要求机器实例能部署Node.js框架)。

<img src=""></img>
<p style="margin-left:100px;">Node做服务端渲染的优缺点</p>

目录规范如下：

```html
src
├── components
│   ├── Item.vue
│   ├── Home.vue
│   └── 404.vue
├── App.vue
├── app.js # 通用 entry(universal entry)
├── entry-client.js # 仅运行于浏览器
└── entry-server.js # 仅运行于服务器
└── router.js # 路由
└── store.js 
```

对于客户端应用程序和服务器应用程序，我们都要使用 webpack 打包 - 服务器需要「服务器 bundle」然后用于服务器端渲染(SSR)，而「客户端 bundle」会发送给浏览器，用于混合静态标记。
<img src=""></img>
<p style="margin-left:400px;">vue-ssr实现原理图</p>

入口模板App.vue 模板中根元素具有 `id="app"`

```js
// App.vue
<template>
    <div id="app">
        <router-view></router-view>
    </div>
</template>
```

由于在Node中创建多个相同服务进程会造成很大资源浪费，故此暴露一个可以重复执行的工厂函数，为每个请求创建新的应用程序实例：

```js
// app.js
import Vue from 'vue'
import App from './App.vue'
import { createRouter } from './router'
import { createStore } from './store'
import { sync } from 'vuex-router-sync'

export function createApp () {
  // 创建 router 和 store 实例
  const router = createRouter()
  const store = createStore()

  // 同步路由状态(route state)到 store
  sync(store, router)

  // 创建应用程序实例，将 router 和 store 注入
  const app = new Vue({
    router,
    store,
    render: h => h(App)
  })

  // 暴露 app, router 和 store。
  return { app, router, store }
}
```

在客户端进行数据预取：

```js
// entry-client.js

import { createApp } from './app'

const { app, router, store } = createApp()

if (window.__INITIAL_STATE__) {
  store.replaceState(window.__INITIAL_STATE__)
}

router.onReady(() => {
  // 添加路由钩子函数，用于处理 asyncData.
  // 在初始路由 resolve 后执行，
  // 以便我们不会二次预取(double-fetch)已有的数据。
  // 使用 `router.beforeResolve()`，以便确保所有异步组件都 resolve。
  router.beforeResolve((to, from, next) => {
    const matched = router.getMatchedComponents(to)
    const prevMatched = router.getMatchedComponents(from)

    // 我们只关心非预渲染的组件
    // 所以我们对比它们，找出两个匹配列表的差异组件
    let diffed = false
    const activated = matched.filter((c, i) => {
      return diffed || (diffed = (prevMatched[i] !== c))
    })

    if (!activated.length) {
      return next()
    }

    // 这里如果有加载指示器 (loading indicator)，就触发

    Promise.all(activated.map(c => {
      if (c.asyncData) {
        return c.asyncData({ store, route: to })
      }
    })).then(() => {

      // 停止加载指示器(loading indicator)

      next()
    }).catch(next)
  })

  app.$mount('#app')
})


```

在服务器端进行数据预取：
在 entry-server.js 中，我们可以通过路由获得与 router.getMatchedComponents() 相匹配的组件，如果组件暴露出 asyncData，我们就调用这个方法。然后我们需要将解析完成的状态，附加到渲染上下文(render context)中。

```js
// entry-server.js
import { createApp } from './app'

export default context => {
  return new Promise((resolve, reject) => {
    const { app, router, store } = createApp()

    router.push(context.url)

    router.onReady(() => {
      const matchedComponents = router.getMatchedComponents()
      if (!matchedComponents.length) {
        return reject({ code: 404 })
      }

      // 对所有匹配的路由组件调用 `asyncData()`
      Promise.all(matchedComponents.map(Component => {
        if (Component.asyncData) {
          return Component.asyncData({
            store,
            route: router.currentRoute
          })
        }
      })).then(() => {
        // 在所有预取钩子(preFetch hook) resolve 后，
        // 我们的 store 现在已经填充入渲染应用程序所需的状态。
        // 当我们将状态附加到上下文，
        // 并且 `template` 选项用于 renderer 时，
        // 状态将自动序列化为 `window.__INITIAL_STATE__`，并注入 HTML。
        context.state = store.state

        resolve(app)
      }).catch(reject)
    }, reject)
  })
}
```

异步路由组件的路由配置示例：
```js
// router.js
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router)

export function createRouter () {
  return new Router({
    mode: 'history',
    routes: [
      { path: '/', component: () => import('./components/Home.vue') },
      { path: '/item/:id', component: () => import('./components/Item.vue') }
    ]
  })
}
```

使用官方状态管理库 Vuex。根据 id 获取 item 的逻辑demo如下：

```js
// store.js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

// 假定我们有一个可以返回 Promise 的
// 通用 API（请忽略此 API 具体实现细节）
import { fetchItem } from './api'

export function createStore () {
  return new Vuex.Store({
    state: {
      items: {}
    },
    actions: {
      fetchItem ({ commit }, id) {
        // `store.dispatch()` 会返回 Promise，
        // 以便我们能够知道数据在何时更新
        return fetchItem(id).then(item => {
          commit('setItem', { id, item })
        })
      }
    },
    mutations: {
      setItem (state, { id, item }) {
        Vue.set(state.items, id, item)
      }
    }
  })
}
```

我们需要通过访问路由，来决定获取哪部分数据 - 这也决定了哪些组件需要渲染。在路由组件上暴露出一个自定义静态函数 `asyncData`，进行`数据预取`：

```js
<!-- Item.vue -->
<template>
  <div>{{ item.title }}</div>
</template>

<script>
export default {
  asyncData ({ store, route }) {
    // 触发 action 后，会返回 Promise
    return store.dispatch('fetchItem', route.params.id)
  },
  computed: {
    // 从 store 的 state 对象中的获取 item。
    item () {
      return this.$store.state.items[this.$route.params.id]
    }
  }
}
</script>
```

与服务器集成：
在Node.js中使用Koa.js(web 框架)，Koa通过利用 async 函数，Koa 帮你丢弃回调函数，并有力地增强错误处理。如下：

```js
import Koa from 'koa';
const app = new Koa();


app.use(async ctx => {
  ctx.body = 'Hello World';
});

app.listen(3000);
```

```html
<!--/views/layout.html-->
<!DOCTYPE html>
<html>
    <head>
        <title>百度糯米</title>
    </head>
    <body>
        <!--vue-ssr-outlet-->
    </body>
</html>
```

使用vue-server-renderer插件

```js
import fs from 'fs';
import LRU from 'lru-cache';
import {createBundleRenderer} from 'vue-server-renderer';

import Koa from 'koa';
const app = new Koa();

const serverBundle = require('../vue-ssr-server-bundle.json');
const clientManifest = require('../vue-ssr-client-manifest.json');
const template = fs.readFileSync(resolve(__dirname, '../../views/layout.html'), 'utf-8');

    let renderer;


    function createRenderer (bundle, options) {
        return createBundleRenderer(bundle, Object.assign(options, {
            template,
            inject: true,
            runInNewContext: false,
            cache: LRU({
                max: 1000,
                // 15min
                maxAge: 1000 * 60 * 15
            })
        }));
    }
    renderer = createRenderer(serverBundle, {
        clientManifest
    });


    app.use(async (ctx, next) => {
        let context = {
            url: ctx.req.url,
            locals: ctx.locals,
            cookie: ctx.req.headers.cookie
        };
        let promise = new Promise((resolve, reject) => {
            renderer.renderToString(context, (err, html) => {
                if (err) {
                    reject(err);
                }
                else {
                    resolve(html);
                }
            });
        });
        try {
            ctx.body = await promise;
        } catch(err) {
            if (err.url) {
                ctx.redirect(err.url);
            }
            else if (err.code === 404) {
                ctx.throw(404, '404 | Page Not Found');
            }
            else {
                // Render Error Page or Redirect
                ctx.throw(500, '500 | Internal Server Error');
            }

        }
    });

    app.listen(8080);

```

### 五、Atom，适用于搜索业务的渲染框架

18年中，[旧百度百聘PC站](https://zhaopin.baidu.com/oldindex)使用Smarty模板+JQuery.js(DOM操作js库)模式开发，大量堆积的DOM操作事件积重难返，对开发维护不是很友好，[重构后网站域名](https://zhaopin.baidu.com)以及各URL均保持一致。为了与百聘wise站技术框架保持一致，重构采用了大搜团队维护的[Atom]框架，Atom 是适用于搜索业务的渲染框架，它提供了与 Vue.js 类似的模版语法以及基于 PHP 的服务器端渲染。
<img src=""></img>
<p style="margin-left:300px;">Atom基础库架构</p>

Atom 提供了:

1. 基于 PHP 的 SSR（后端架构不需要迁移 node，成本低），性能优于`node + vue`
2. 和Vue 类似的模版语法
3. 组件化的开发方式，可以对 html、css代码进行模块化管理
4. MVVM 数据驱动
5. 前后端同构，方便做一系列的渲染优化

目录规范如下：

```html
├── api
│   └── fav.js                     // 本地开发mock ajax请求返回.例子：职位收藏接口
├── package.json                   // npm依赖
├── gulpfile.js                    // gulp针对本地 构建的配置文件
├── gulpfile-rd.js                 // gulp针对RD  构建的配置文件
├── BCLOUD
├── build.sh                       // 构建用的 shell
├── tools                          // 所有的本地脚本在这个目录
│   ├── template.php               // 所有页面的入口模板 template for page entry
│   ├── AtomWrapper.class.php      // 渲染 atom php 文件用的脚本
│   ├── get-mock-data.js           // 用于生成 mock 数据
│   ├── webpack.prod.js            // webpack configuration for production
│   ├── webpack.dev.js             // webpack configuration for dev
│   ├── routes.json                // 本地开发的路由配置
│   ├── router.php                 // 路由
│   ├── php-cgi.js                 // run php script through php-cgi 
│   ├── server.js                  // 本地开发服务器的入口 express server for development
│   ├── server.php                 // 地开发服务器
│   ├── template.js                // create template to serve every entry with ssr support
│   ├── ajax-middleware.js         // ajax handler for dev
│   └── entry-loader.js            // Entry Loader
│
├── src                            // 所有的源码在这里
│   ├── Home                       // 建议每个页面都用一个目录来承载
│   │   ├── child.atom             // 自有 atom 组件
│   │   ├── index.atom             // 入口 atom 组件[关键][必须]
│   │   ├── index.mock.js          // mock 数据文件，方便FE本地mock开发
│   │   └── data_modify.php        // 建议在这个文件中进行数据结构优化处理
│   ├── common                     // 在多个页面中公共使用的模块
│   │    ├── component             // 公共组件
│   │    │   └── app.atom          // 布局组件
│   │    ├── img                   // 图片资源
│   │    ├── utils                 // 工具函数模块
│   │    │   ├── log.js
│   │    │   └── ...
│   │    └── index.js              // 整站唯一的入口文件
│   └── ...                         // 其他页面...
└── 其他

```

入口 atom：
入口 atom 我们是依靠约定 `index.atom` 为入口来进行标识的，即所有的 `index.atom` 都是一个页面的入口，*.atom依靠
结合/src/common/index.js和对应页面的数据预处理文件data_modify.php:

```js
// /src/common/index.js
import Atom from 'atom';
import './global.css';
import './icon.css';

export function init(MainComponent, data, props) {
    new Atom({
        el: '[atom-root]',
        data: data,
        components: {
            app: MainComponent
        },
        render(createElement) {
            let a = props.reduce(
                (props, prop) => {
                    props[prop] = this[prop];
                    return props;
                },
                {}
            );
            return createElement(
                'app',
                {
                    props: a
                }
            );
        }
    });
}

export function createEntry(Component) {
    return function (...args) {
        init(Component, ...args);
    };
}

```

data_modify.php示例如下：

```php
<?php

$city = $this->data['data']['city_name'];
$title = '【找工作_人才招聘_'.$city.'招聘信息】';
$seoDesc = '全职招聘，名企推荐，靠谱兼职，招聘会，校园招聘，职场资讯，宣讲会';
$keyWords = $city.'招聘网|'.$city.'招聘信息|'.$city.'人才网|'.$city.'热门工作';
$this->modify_data["title"] = $title;
$this->modify_data['seoDesc'] = $seoDesc;
$this->modify_data['keyWords'] = $keyWords;

$this->modify_data['city'] = $city;
$this->modify_data['tplData'] = $this->data['data'];


```

Atom 采用 Single File Component 的形式进行组件的管理，一个 .atom 文件可以定义一个 Atom 组件。
.atom 文件包含五部分：
1. template：模版部分，组件的 html（或 xml） 结构
2. config：组件的数据、依赖等相关内容，服务器端和浏览器端都会使用
3. script：前端逻辑，只在浏览器端使用
4. php：服务器端逻辑，只在 php 的服务器端使用，很少会用到
5. style：组件样式

index.atom示例如下：

```js
<template>
    <div class="home-body">
        <!-- 头部 -->
        <site-header :curcity="city" />

        <!-- main -->
        <main-content />

        <!-- 事件 -->
        <div @click="clickHandler" class="simple"></div>
        <!-- 尾部 -->
        <site-footer />
    </div>
</template>



<script type="config">
{
    props: {
        tplData: {
            type: Object,
            default: {}
        }
    },
    data: {
        city: '北京',
    },
    components: {
        'site-header': '../common/component/header.atom',
        'main-content': '../common/component/main.atom',
        'site-footer': '../common/component/footer.atom'
    }
}
</script>

<script type="php">

function computed_url($ctx) {
    $c = $ctx->_d['curcity'];
    return '/quanzhi?city=' . $c . '&query=';
}

</script>

<script>
import { getQueryValue, getCookie } from '../common/utils/util';

module.exports = {
    mounted() {
        this.city = getQueryValue('city') || this.tplData.city_name;
    },
    methods: {
        clickHandler: function () {}
    }
};
</script>


<style lang="less" scoped>
.simple
    font-size: 24px;
}
</style>
```

使用atom编译插件`atom-loader`来生成`*.atom.php`：

```js
// webpack配置片段：
    rules: [
            {
                test: /index\.atom$/,
                use: [
                    'babel-loader',
                    path.resolve('tools/entry-loader.js')
                ]
            },
            {
                test: /\.js$/,
                use: [
                    'babel-loader'
                ],
                include: path.resolve(__dirname, '../src')
            },
            {
                test: /\.atom$/,
                use: [
                    {
                        loader: 'atom-loader',
                        options: {
                            compile: {
                                compileJSComponent(val, key) {
                                    return `require("${val}")`;
                                },
                                compilePHPComponent(val, key) {
                                    return ''
                                        + 'dirname(__FILE__) . "/" '
                                        + '. ' + JSON.stringify(val + '.php');
                                },
                                compileStyle: atomStyleCompiler
                            },
                            resolvePhpOutputPath(filePath) {
                                let outputFilePath = filePath
                                    .replace(root, '.')
                                    .replace('src', 'output/template');
                                return `${outputFilePath}.php`;
                            },
                            loaders: {
                                css: ExtractTextPlugin.extract({
                                    fallback: 'style-loader',
                                    use: [
                                        'css-loader',
                                        'less-loader',
                                        {
                                            loader: 'postcss-loader',
                                        }
                                    ]
                                })
                            }
                        }
                    }
                ]
            }
        ]
```

通用模板template.php如下：

```php
<?php /* template for page entry */ ?>
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title><?php echo htmlentities($data['title']?></title>
    <meta name="description" content="<?php echo htmlentities($data['seoDesc'])?>"/>
    <meta name="keywords" content="<?php echo htmlentities($data['keyWords'])?>"/>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <link rel="shortcut icon" href="https://zhaopin.baidu.com/static/newjobs/img/logo/baipin_icon.png">
</head>
<body data-click="{'act': 'as'}">
<div [atom-root]>
<?php echo $atom['html'] ?>
<script>
var props = window.__COMPONENT_PROPS__ = <?php echo json_encode($atom['props']) ?>;
var data = window.__DATA__ = {};
<?php
if ($atom['props'] != null && !empty($atom['props'])) {
    foreach ($atom['props'] as $value) {
        if (isset($data[$value])) {
            echo "data[" . json_encode($value) . "] = " . json_encode($data[$value]) . ";\n";
        }
    }
}
?>
</script>
</div>
</body>
</html>
```

缺点是Atom不支持IE8及以下浏览器，理由和Vue.js类似，因为 Vue.js 使用了 IE8 不能模拟的 ECMAScript 5 特性-`Object.defineProperty()`。

### 六、百度小程序

2017年1月9日，[微信小程序](https://developers.weixin.qq.com/miniprogram/dev/)上线，目前微信小程序已超过100w个；2018年7月4日北京国家会议中心，百度正式发布[智能小程序](https://smartprogram.baidu.com)；2017年8月18日[支付宝小程序](https://docs.alipay.com/mini/developer/getting-started)开启了内测，9月20日正式对外开放。
<img src=""></img>
<p style="margin-left:260px;">轻应用之小程序</p>

这里我们主要讲我们的百度小程序。百度小程序依托以`百度App`为代表的全域流量，通过百度AI开放式赋能， 精准连接用户，无需下载安装便可享受智慧超前的使用体验。


<img src=""></img>
<p style="margin-left:260px;">Demo：手百信息流接入贴吧小程序</p>

一般，在原生小程序语法的开发模式下，目录规范如下：

```json
|____app.css // 全局通用样式
|____app.json // 设置 SWAN 的界面、路径、多 TAB
|____project.swan.json // 设置小程序APP ID等
|____pages
|       |____detail
|       |        |____detail.css
|       |        |____detail.swan // 模板文件
|       |        |____detail.js
|       |        |____detail.json // 页面标题栏配置、组件引用等声明
|       |____index
|       |        |____index.css
|       |        |____index.swan
|       |        |____index.js
|       |        |____index.json
|____app.js // 全局的 JS 逻辑
```

我们看到以下几类的文件：
1、 .json 为后缀的 JSON 配置文件，这个文件配置了 SWAN 智能小程序所有页面的路径和界面展现样式等；
2、 .swan 结尾的 SWAN 模板文件，这个文件是用来描述当前这个页面的文件结构，类似于网页中的 HTML 文件；
3、 .css 结尾的 CSS 样式文件，描述页面样式；
4、 .js 结尾的 JS 文件，处理这个页面和用户的交互。
小程序在技术架构上非常清晰易懂。JS负责业务逻辑的实现，而表现层则SWAN和CSS来共同实现，SWAN其实就是一种微信定义的模板语言。所以对于擅长前端开发，或者WEB开发的广大开发者而已，小程序的开发可谓降低了不少门槛。
<img src=""></img>
<p style="margin-left:400px;">小程序架构图</p>

从上面的小程序架构图上可以清晰的看出，小程序借助的是JSBridge实现了对底层API接口的调用，所以在小程序里面开发，开发者不用太多去考虑IOS，安卓的实现差异的问题，安心在上层的视图层和逻辑层进行开发即可。
<img src="" width="600" height="400"></img>
<p style="margin-left:260px;">小程序借助JSBridge</p>

目前有『搬家工具』可以让百度小程序和微信小程序『相互转换』，给出差异log，端能力API转换等，但仍不成熟。目前有不少应用层框架支持跨端小程序开发,用类现代框架 (vue/react) 的语法去开发小程序，，比如[Taro](https://nervjs.github.io/taro/docs/README.html)、[okam](https://ecomfe.github.io/okam/#/)，[mars](https://max-team.github.io/Mars/)提升开发体验和解决跨平台的问题，这个后续我们可以研究一下。

## 总结

项目的开发模式，取决于团队技术栈是什么样组合，能用什么技术框架，结合产品特点，比如面向用户是谁，PC端是否需要兼容IE，是否需要SEO等等，同时利用组件化、模块化支持大型项目分工协助，最后利用构建工具的自动化，最终打造出多种高效的协助开发模式。

## 参考

[fis](http://fis.baidu.com)
[前端开发模式的迭代](https://mp.weixin.qq.com/s/zo2Ke-Pmd_PCmAtUU6HdUw)
[Hybrid APP基础篇(一)->什么是Hybrid App](https://www.cnblogs.com/dailc/p/5930231.html)
[Hybrid APP基础篇(二)->Native、Hybrid、React Native、Web App方案的分析比较](http://www.cnblogs.com/dailc/p/5930238.html)
[nginx配置location总结及rewrite规则写法](https://segmentfault.com/a/1190000002797606)
[什么是前端](https://www.cnblogs.com/erxiaojiedeah/p/9907339.html)
[百度Web前端开发实战案例解析](https://mp.weixin.qq.com/s?__biz=MjM5MDE0Mjc4MA==&mid=2651010393&idx=2&sn=b94ae823d621741e853a14ad540ef993)

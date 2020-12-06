---
title: "AngularJS:select内置指令用法详解"
date: 2016-04-28 17:00:00
tags: ['AngularJS', '内置指令']
description: "一直以为select内置指令只有一种用法。少年, 多看看官网, 即使maybe, 翻墙才行."

---
[原文转自蛋糕仙人：](http://gejiawen.github.io/)

---

select指令是 AngularJS 预设的一组directive。这里是AngularJS官网给出的用法： [AngularJS:select](http://docs.angularjs.org/api/ng.directive:select)

大概的意思是，select中的ngOption可以采用和ngRepeat指令类似的循环结构，其数据源可以是Array或者是Object。针对两种数据源又可以衍生出好几种用法。

但是官网给出的例子太少了。

下面是针对几个不太容易理解的用法示例。

## 先上Controller

```js

function selectCtrl($scope) {
    $scope.selected = '';

    $scope.model = [{
        id: 10001,
        mainCategory: '男',
        productName: '水洗T恤',
        productColor: '白'
    }, {
        id: 10002,
        mainCategory: '女',
        productName: '圆领短袖',
        productColor: '黑'
    }, {
        id: 10003,
        mainCategory: '女',
        productName: '短袖短袖',
        productColor: '黄'
    }];
}

```

## 示例
### 示例一：基本下拉效果
usage:
label for value in array

```js
<select ng-model="selected" ng-options="m.productName for m in model">
    <option value="">-- 请选择 --</option>
</select>
```

示例二：自定义下拉显示名称
usage
label for value in array
```js
<select ng-model="selected" ng-options="(m.productColor + ' - ' + m.productName) for m in model">
    <option value="">-- 请选择 --</option>
</select>
```

示例三：让选项分组
usage
label group by groupName for value in array
```js
<select ng-model="selected" ng-options="(m.productColor + ' - ' + m.productName) group by m.mainCategory for m in model">
    <option value="">-- 请选择 --</option>
</select>
```

示例四：自定义ngModel的绑定
usage
select as label for value in array
```js
<select ng-model="selected" ng-options="m.id as m.productName for m in model">
    <option value="">-- 请选择 --</option>
</select>
```
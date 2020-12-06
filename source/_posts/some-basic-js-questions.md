---
title: "Javascript中容易忽略的问题"
date: 2016-04-12 16:14:00
tags: ['Javascript', '做题']
description: "请问， 你觉得0.1 + 0.2 等于多少？"


---

### (1) 0.1 + 0.2 = ?
你是否觉得一眼看上去，这么简单，不就是等于0.3。图样图破森
```js
console.log(0.1 + 0.2)  // 0.30000000000000004

```
JS的浮点数精度问题：
1 请在代码中浮点数先转换为整数进行运算，最后再除以倍数：
针对一位浮点数的运算，用<pre>(0.1 * 10 + 0.7 * 10) / 10</pre>>代替<pre>0.1 + 0.7</pre>，
针对两位浮点数的运算，用<pre>(0.13 * 100 + 0.76 * 100) / 100</pre>>代替<pre>0.13 + 0.76</pre>，同理可推
2 利用toFixed(args)知道浮点数的小数位数

### (2) 看题说话
```js
(function(){
    var x=y=1;
})()
console.log(x);
console.log(y); 
```
这里应该考虑到闭包，隐式创建全局变量等问题

```js
var undefined;
undefined == null; 
1 == true;   
2 == true;   
0 == false;  
0 == '';     
NaN == NaN;  
[] == false; 
[] == ![];  
```
平时判断过一个对象（值）是否为空吧，仔细考虑一下这些结果吧。

那么，接着，
```js
var foo = "11"+2-"1";
console.log(foo); 
console.log(typeof foo); 

var foo2 = 11 +2 -'1';
console.log(foo2);
console.log(typeof foo2);

var foo3 = 11 +2 +'1';
console.log(foo3);
console.log(typeof foo3);


var foo4 = '1'*2+"1";
console.log(foo4);
console.log(typeof foo4);

var foo5 = '1'*2-"3";
console.log(foo5);
console.log(typeof foo5);
```
 这里应该考虑到隐式转换等技巧。

```js
var a = new Object();
a.value = 1;
b = a;
b.value = 2;
alert(a.value);
```

```js
var foo = 1;
(function(){
    console.log('test1', foo);
    var foo = 2;
    console.log('test2', foo);
})()
```
这里应该考虑到变量提升、变量作用域等问题。

### 技巧之转换
```js
var a=0;!!a   // false

var b=1;!!b   // true

var c='';!!c  // false

var d='0';!!d // true
b_bool = !!myVar,  /*  to boolean - any string with length and any number except 0 are true */

```
基础变量的前面增加<code>!!</code>的时候，会转换成为boolean值，不为空的字符串以及值不为0的数字都会转换为**true**。

```js
var a = 11;
var b; 
b = +a;
console.log(b, typeof b);  // 11 "number"

var a2 = '11';
var c;
c= +a2
console.log(c, typeof c); // 11 "number"
```

变量为数字或者可以转换为数字的数字字符串的，前面增加<code>+</code>,可以转换为数字。
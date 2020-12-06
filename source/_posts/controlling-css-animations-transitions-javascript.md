---
title: "如何使用JavaScript控制CSS Animations和Transitions"
date: 2016-05-09 20:04:00
tags: ['Javascript', '技巧']
description: "注意：Animations(动画)和Transitions(过渡)是不同的;CSS Transitions(过渡)被应用于元素指定的属性变化时，该属性经过一段时间逐渐的过渡到最终需要的值；而CSS Animations(动画)只是在应用时执行之前定义好的操作，它提供更细粒度的控制。"


---
[原文转自w3cplus：](http://www.w3cplus.com/animation/controlling-css-animations-transitions-javascript.html)

CSS animations和transitions再加上点JavaScript就可以实现硬件加速动画，而且其交互效果比大多数JavaScript库更高效。 So,让我们快点开始吧！小伙伴们都等不及了！

### 注意：Animations(动画)和Transitions(过渡)是不同的

CSS Transitions(过渡)被应用于元素指定的属性变化时，该属性经过一段时间逐渐的过渡到最终需要的值；而CSS Animations(动画)只是在应用时执行之前定义好的操作，它提供更细粒度的控制。

在这篇文章中，我们将分别针对上述内容进行讲解。

### 控制CSS Transition(过渡)

在编程论坛中，关于transition(过渡)的触发和暂停有无数的疑问。使用JavaScript可以很容易的解决这些疑问。

触发元素的transiton(过渡)，切换元素的类名可以触发该元素的transition(过渡)

暂停元素的transition(过渡)， 在你想要暂停过渡点，用getComputedStyle和getPropertyValue获取该元素相应的CSS属性值，然后设置该元素的对应的CSS属性等于你刚才获取到的CSS属性值。

以下是该方法的一个例子。

HTML:
```js
<h3>Pure Javascript</h3>
<div class='box'></div> 
<button class='toggleButton' value='play'>Play</button>

<h3>jQuery</h3>
<div class='box'></div> 
<button class='toggleButton' value='play'>Play</button>
```

css:

```js
.box {
  margin: 30px;
  height: 50px;
  width: 50px;
  background-color: blue;
}
.box.horizTranslate {
  -webkit-transition: 3s;
  -moz-transition: 3s;
  -ms-transition: 3s;
  -o-transition: 3s;
  transition: 3s;
  margin-left: 50% !important;
}
```

JS:
```js
HTML  CSS  JS  Result
Edit on 
var boxOne = document.getElementsByClassName('box')[0],
    $boxTwo = $('.box:eq(1)');

document.getElementsByClassName('toggleButton')[0].onclick = function() {
  if(this.innerHTML === 'Play') 
  { 
    this.innerHTML = 'Pause';
    boxOne.classList.add('horizTranslate');
  } else {
    this.innerHTML = 'Play';
    var computedStyle = window.getComputedStyle(boxOne),
        marginLeft = computedStyle.getPropertyValue('margin-left');
    boxOne.style.marginLeft = marginLeft;
    boxOne.classList.remove('horizTranslate');    
  }  
}

$('.toggleButton:eq(1)').on('click', function() { 
  if($(this).html() === 'Play') 
  {
    $(this).html('Pause');
    $boxTwo.addClass('horizTranslate');
  } else {
    $(this).html('Play');
    var computedStyle = $boxTwo.css('margin-left');
    $boxTwo.removeClass('horizTranslate');
    $boxTwo.css('margin-left', computedStyle);
  }  
});
```
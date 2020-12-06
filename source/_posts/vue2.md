---
title: "vue2原理浅析"
date: 2019-03-09 13:50:00
tags: ['vue', 'vue源码解读']
description: "鼓起勇气粗略读了一遍vue2.0源码，再结合其他前端大神的博客，对vue2.0有了一定理解。"

---
参考[这篇文章](https://imhjm.com/article/59b902107dd03248a2e8d584)

### vue是怎么实现数据双向绑定的
vue2.0是通过Object.defineProperty对数据进行『劫持』，现在我们知道vue3.0是通过proxy实现在这里就不讲了。
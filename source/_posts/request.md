---
title: "前端请求：Ajax、fetch、Axios"
date: 2019-02-12 17:50:00
tags: ['Ajax', 'fetch', 'Axios']
description: "Ajax可以实现无刷新的数据交换，让用户的操作更流畅。"
---

### Ajax
通常指JQuery Ajax，是对原生XMLHttpRequest（XHR）的封装，同时还增加JSONP的支持。
```js
// 原生XHR
var xhr = new XMLHttpRequest();
xhr.open('GET', url);
xhr.onreadystatechange = function() {
    if (xhr.readyState === 4 && xhr.status === 200) {
        console.log(xhr.responseText)   // 从服务器获取数据
    }
};
xhr.send();
```


```js
基于jquery
$.ajax({
    type: 'POST',
    url: url,
    data: data,
    dataType: dataType,
    success: function(data) {
		console.log('从服务器成功获取数据:', data);
	},
    error: function(err) {
		console.log('从服务器获取数据异常:', err);
	}
});
```

### fetch
fetch号称是ajax的替代品，它的API是基于Promise设计的。

```js
fetch(url)
    .then(response => {
        if (response.ok) {
            response.json()
        }
    })
    .then(data => console.log(data))
    .catch(err => console.log(err))
```

### Axios
Axios本质上也是对原生XHR的封装，只不过它是Promise的实现版本，符合最新的ES规范，同时支持浏览器和 nodejs。

```js
axios({
    method: 'GET',
    url: url,
})
.then(res => {console.log(res)})
.catch(err => {console.log(err)})
```

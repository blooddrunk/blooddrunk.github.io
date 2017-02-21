---
title: CSS sticky position 及用纯javascript的polyfill 
tags:
  - CSS
categories:
  - 前端
  - CSS
date: 2017-02-18 13:50:18
---

已经有很多浏览器开始支持CSS属性[postion: sticky](http://caniuse.com/#feat=css-sticky)。它是`relative`和`fixed`的`position`属性结合。被指定为`position: sticky;`的元素刚开始被视为`relative`，当页面滚动要将其移出viewport时，会变为'fixed'。有了它，以前我们需要借用插件或者JS才能实现的*Sticky*效果只需一行CSS即可实现：

<!-- more -->

{% codepen jelinenaro RKmybx 0 result 300 %}

如果使用javascript，最简单的做法可能是(https://www.sitepoint.com/css-position-sticky-introduction-polyfills/)

```javascript
var menu = document.querySelector('.menu')
var menuPosition = menu.getBoundingClientRect().top;
window.addEventListener('scroll', function() {
    if (window.pageYOffset >= menuPosition) {
        menu.style.position = 'fixed';
        menu.style.top = '0px';
    } else {
        menu.style.position = 'static';
        menu.style.top = '';
    }
});
```

可是仔细观察的话，在元素从`static`变为`fixed`的一瞬间，由于它脱离了正常的文档流，会向上“跳过”几个像素的高度：

{% codepen jelinenaro pRmVoX 0 result 300 %}

这个高度大约等于该元素本身的高度(与元素本身的`margin`, `border`等盒模型属性相关)。一个可能的解决方法是，手动插入一个和该元素大小相等的展位元素。下面是改进的代码:

```javascript
var menu = document.querySelector('.menu');
var menuPosition = menu.getBoundingClientRect();
var placeholder = document.createElement('div');
placeholder.style.width = menuPosition.width + 'px';
placeholder.style.height = menuPosition.height + 'px';
var isAdded = false;

window.addEventListener('scroll', function() {
    if (window.pageYOffset >= menuPosition.top && !isAdded) {
        menu.classList.add('sticky');
        menu.parentNode.insertBefore(placeholder, menu);
        isAdded = true;
    } else if (window.pageYOffset < menuPosition.top && isAdded) {
        menu.classList.remove('sticky');
        menu.parentNode.removeChild(placeholder);
        isAdded = false;
    }
});
```

问题解决了:

{% codepen jelinenaro qRGYbr 0 result 300 %}


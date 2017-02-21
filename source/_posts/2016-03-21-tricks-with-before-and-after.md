---
title: 伪元素::before ::after的一些例子
tags:
  - CSS
categories:
  - 前端
  - CSS
date: 2016-03-21 23:11:41
---

## Tooltips

绝对定位和`attr`方法

{% codepen jelinenaro VPoXMZ 0 result 200 %}

<!-- more -->

## Counter

CSS *Counter*本质上是由CSS维护的用来跟踪某些CSS定义被使用次数的变量。使用*Counter*可以使用纯CSS实现各种丰富的分段计数等效果。使用到的属性:

* `counter-reset` - 创建或者重置一个counter
* `counter-increment` - 增加一个counter的数字值
* `content` - 在页面中插入生成的内容
* `counter()` 或者 `counters()` 函数 - 返回某个counter的值

> `counter()` 函数可以接受第二个参数，参数值同 `list-style-type` 的值，从而可以让 counter 的值不只是数字:
  * disc (• • •)
  * circle (○ ○ ○)
  * square (▪ ▪ ▪)
  * decimal (1 2 3)
  * decimal-leading-zero (01, 02, 03)
  * lower-roman (i ii iii)
  * upper-roman (I II III)
  * lower-greek (α β γ)
  * lower-latin (a b c)
  * upper-latin (A B C)
  * armenian (Ա Բ Գ)
  * georgian (ა ბ გ)
  * lower-alpha (a b c)
  * upper-alpha (A B C)

{% codepen jelinenaro bgXvZY 0 result 200 %}

使用 `counters()` 的多级标题

{% codepen jelinenaro rjXvJN 0 result 200 %}


## img

> https://bitsofco.de/styling-broken-images/

图片加载失败时，提供更生动的显示效果

{% codepen jelinenaro YNmLOr 0 result 200 %}
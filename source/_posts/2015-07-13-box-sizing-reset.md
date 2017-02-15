---
title: box-sizing reset
tags:
  - CSS
categories:
  - 前端
  - CSS
date: 2015-07-13 23:19:01
---

在网站中使用`border-box`的[`box-sizing`](https://css-tricks.com/almanac/properties/b/box-sizing/)可以简化我们计算元素布局尺寸时的工作，下面是一种较为流行的reset做法

```css
html {
  box-sizing: border-box;
}
*, *:before, *:after {
  box-sizing: inherit;
}
```
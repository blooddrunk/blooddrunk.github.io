---
title: 边距塌陷(组合) margin collapse
tags:
  - CSS
categories:
  - 前端
  - CSS
date: 2016-02-09 23:06:33
---

块元素的上/下边距有时会塌陷(组合)成一个单的的边距，实际的边距大小是两个边距中值最大的一个，这就是所谓的*margin collapsing*

最常见的情况出现在上下相邻的两个元素，如下，虽然上面的`div`有`25px`的下边距，但由于下面的`div`的上边距为`50px`，所以两者的实际间距为`50px`

{% codepen jelinenaro ggBzbL 0 result 350 %}
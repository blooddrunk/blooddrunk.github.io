---
title: CSS属性顺序惯例
tags:
  - CSS
categories:
  - cheat sheet
date: 2016-04-13 22:48:05
---

CSS的[style guide](https://github.com/airbnb/css#ordering-of-property-declarations)有很多，重度强迫症也许对属性的排序也有要求，大部分人倾向于按类型/重要性排序， 比如按下面的分类来排序(分组)：

* Layout Properties (`position`, `float`, `clear`, `display`)
* Box Model Properties (`width`, `height`, `margin`, `padding`)
* Visual Properties (`color`, `background`, `border`, `box-shadow`)
* Typography Properties (`font-size`, `font-family`, `text-align`, `text-transform`)
* Misc Properties (`cursor`, `overflow`, `z-index`)

<!-- more -->

```css
.selector {
  /* Positioning */
  position: absolute;
  z-index: 10;
  top: 0;
  right: 0;

  /* Display & Box Model */
  display: inline-block;
  overflow: hidden;
  box-sizing: border-box;
  width: 100px;
  height: 100px;
  padding: 10px;
  border: 10px solid #333;
  margin: 10px;

  /* Color */
  background: #000;
  color: #fff;
  
  /* Text */
  font-family: sans-serif;
  font-size: 16px;
  line-height: 1.4;
  text-align: right;

  /* Other */
  cursor: pointer;
}
```
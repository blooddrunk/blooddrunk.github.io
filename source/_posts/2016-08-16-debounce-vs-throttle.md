---
title: debounce vs throttle
tags:
  - Javascript
categories:
  - 前端
  - Javascript
date: 2016-08-16 21:49:46
---

> https://css-tricks.com/debouncing-throttling-explained-examples/

[Debounce](https://lodash.com/docs/4.17.4#debounce)和[Throttle](https://lodash.com/docs/4.17.4#throttle)是用来控制一段时间内某个函数执行次数的类似但不相同的两种技巧。如果你曾想要控制一个`scroll`事件的触发频率，你很可能考虑过到底该用两者中的哪一个。

<!-- more -->

{% codepen jelinenaro XpQREo 0 result 300 %}

简单来说：

* **debounce** 将短时间内突然大量爆发的重复事件(比如键盘点击)限制为一个
* **throttle** 保证函数每N毫秒固定执行一次。比如每200ms检查一次页面滚动的位置来触发一个CSS动画

> 在某些情况下，[**requestAnimationFrame**](https://css-tricks.com/using-requestanimationframe/)可发挥和`throttle`类似的作用，比如你要执行的函数用来计算并在屏幕上呈现某些元素，使用`requestAnimationFrame`可以实现更平滑的变化(动画)

## Debounce

```
_.debounce(func, [wait=0], [options={}])
```

按照*Lodash*的解释，[Debounce](https://lodash.com/docs/4.17.4#debounce)将一个函数(`func`)的调用推迟到自上次该**Debounced**函数被调用后的某个时间(`wait`)后。

```javascript
// Avoid costly calculations while the window size is in flux.
jQuery(window).on('resize', _.debounce(calculateLayout, 150));
 
// Invoke `sendMail` when clicked, debouncing subsequent calls.
jQuery(element).on('click', _.debounce(sendMail, 300, {
  'leading': true,
  'trailing': false
}));
 
// Ensure `batchLog` is invoked once after 1 second of debounced calls.
var debounced = _.debounce(batchLog, 250, { 'maxWait': 1000 });
var source = new EventSource('/stream');
jQuery(source).on('message', debounced);
 
// Cancel the trailing debounced invocation.
jQuery(window).on('popstate', debounced.cancel);
```

想象你正在一个电梯中，在电梯门正要关上的时候，另外一个人想要冲进电梯。电梯不会开始运动，而是再次打开门，这个过程会重复直到不再有人想要闯入电梯为止。如果之前**Debounced**函数并没被执行过，那么被延迟的事件就不会被执行。幸运的是，`Debounce`(*Lodash*的实现)有个`maxWait`的配置项控制函数能被延迟执行的最大间隔。

回到最上面的例子，看看用`Debounce`的效果，连续滚动，数字只会在滚动停止后的200ms后执行一次。`Debounce`在这个例子中显然不合适（虽然设置`maxWait`可以解决这个问题)。但是却适合用于`resize`的场合，因为往往我们只知道resize发生了就够了。

> http://stackoverflow.com/questions/24079736/confused-about-the-maxwait-option-for-lodashs-debounce-method

scroll:

{% codepen jelinenaro ZLZydb 0 result 300 %}

resize:

{% codepen jelinenaro wgZqEB 0 result 300 %}


## Throttle

```
_.throttle(func, [wait=0], [options={}])
```

`Throttle`则要好理解很多。[Throttle](https://lodash.com/docs/4.17.4#throttle)确保一个函数(`func`)在一段时间(`wait`)内至多被调用一次

```javascript
// Avoid excessively updating the position while scrolling.
jQuery(window).on('scroll', _.throttle(updatePosition, 100));
 
// Invoke `renewToken` when the click event is fired, but not more than once every 5 minutes.
var throttled = _.throttle(renewToken, 300000, { 'trailing': false });
jQuery(element).on('click', throttled);
 
// Cancel the trailing throttled invocation.
jQuery(window).on('popstate', throttled.cancel);
```

最上面的例子用`Throttle`来实现，是不是正是我们想要的效果

{% codepen jelinenaro XpQaOV 0 result 300 %}

## requestAnimationFrame

使用`requestAnimationFrame`来实现经过浏览器优化的、更省电的动画效果。此外，在不活动的标签页的调用会被暂停以节省资源。

{% codepen jelinenaro OWGNom 0 result 300 %}
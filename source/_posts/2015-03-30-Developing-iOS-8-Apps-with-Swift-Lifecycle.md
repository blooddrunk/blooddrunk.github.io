---
title: 用Swift语言开发iOS8 App： 生命周期
date: 2015-03-30 20:55:50
tags:
  - Swift
  - iOS
---

一个View Controller的生命周期中会调用一系列方法，我们常常需要override这些方法来实现某些功能。

- 初始化(通常是从storyboard开始)
- `awakeFormNib`(每一个来自storyboard的对象都会调用这个方法，包括Controller）
- `prepareForSegue`(为segue做一些准备工作)
- outlet获得值(这意味着在`prepareForSegue`时，`destinationViewController`的outlet还没有值)
- `viewDidLoad`(最常用的放置初始化代码的地方)
- `viewWillAppear` `viewDidAppear`(view出现时调用，可能会调用多次)
- `viewWillDisappear` `viewDidDisappear`(view消失时调用，可能会调用多次)
- `viewWillLayoutSubviews` `viewDidLayoutSubviews`(当一个view的frame改变，需要重新布局时调用，比如呈现、旋转，*autolayout*发生在"will"和"did"之间)
- `didReceiveMemoryWarning`(内存不足，非常罕见的情况)
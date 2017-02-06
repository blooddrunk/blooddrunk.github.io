---
title: Swift的一些标准内置函数
date: 2015-03-27 23:50:15
tags:
  - Jekyll
  - blog
---

Swift有一些全局标准函数在[The Swift Programming Language](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/)并没有提到。在**Paul Hegary**的Swift[课程](https://itunes.apple.com/cn/course/developing-ios-8-apps-swift/id961180099 "Developing iOS 8 Apps with Swift")中看到一些，做下笔记。

{% codeblock lang:swift %}

// Collections包括Array, Dictionary, String
// Sliceables包括Array, String

let sub = dropFirst(aSliceable)                   // 返回一个去掉第一个元素的新序列
let sub = dropLast(aSliceable)                    // 返回一个去掉最后一个元素的新序列
let first = first(aCollection)                    // 返回序列的第一个元素(可选值)
let last = last(aCollection)                      // 返回序列的最后一个元素(可选值)
let prefix = prefix(aSliceable, X: Int)           // 返回序列的前X个元素的子序列
let suffix = suffix(aSliceable, X: Int)           // 返回序列的后X个元素的子序列
let reversed = reverse(aCollection)               // 返回倒序的序列，注意返回值是一个数组
let backwardsString = String(reverse(str))        // 返回字符串的倒序
let isEqual = equal(aSliceable1, aSliceable2)     // 检查两个序列是否相等

{% endcodeblock %}

<!-- more -->

此外正如由于Unicode码元的原因应该用`countElements`来获取字符串的长度，在Swift中也无法简单的用`Int`来表示`String`的Index，取而代之的是`String.Index`，而使用`Range<String.Index>`来表示字符串片段

{% codeblock lang:swift %}

var str = "hello"
let start = str.startIndex                       // 获得字符串的第一个字符的位置
let end = str.endIndex                           // 获得字符串的最后一个字符的位置
let index = advance(str.startIndex, 2)           // 获得字符串第三个字符的位置(第一个字符之后两个)
let substring = str[index..<end]                 // 获得子串

{% endcodeblock %}

上面的提到了`Range`，它表示Swift中序列的一个片段，在`for-in`循环中已经隐式使用过它了：

{% codeblock lang:swift %}

var r: Range<Int> = 1...7                       // 或者用1..<8表示一个半闭区间
for i in r {
    println(i)
}

{% endcodeblock %}

借助`Range<String.Index>`我们可以实现类似IndexOf的效果(不知道有没有简单的方法)：

{% codeblock lang:swift %}

var str = "Hello, playground"
let commaIndex = str.rangeOfString(",")!.startIndex        // 5
let stringBeforeComma = str.substringToIndex(commaIndex)   // "Hello"
str.removeRange(commaIndex..<str.endIndex)                 // str = "Hello"，参数相当于Range<String.Index>(start: commaIndex, end: str.endIndex)

{% endcodeblock %}
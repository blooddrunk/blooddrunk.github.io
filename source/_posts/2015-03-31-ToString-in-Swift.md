---
title: Swift中的"toString"方法
date: 2015-03-31 21:44:51
tags:
  - Swift
---

在Swift为对象实现类似`toString`的方法非常简单：在相应的类、结构或枚举声明中实现`Printable`协议，然后实现该协议指定的`description`只读计算属性即可：

{% codeblock lang:swift %}

struct Rect: Printable {
    var width: Double
    var height: Double
    
    var description: String {
        return "width=\(width), height=\(height)"
    }
}

let r = Rect(width: 11, height: 22)
println("My Rect: \(r)")

{% endcodeblock %}

> 在**playground**中不知道为什么在右侧看到的结果跟没有实现`Printable`协议一样，这应该是playground的问题

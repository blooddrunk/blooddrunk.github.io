---
title: Swift 1.2中类的可失败构造方法
date: 2015-05-10 23:09:42
tags:
  - Swift
---

在Swift的构造函数中可以通过`return nil`来表示构造过程可能失败，如下：

{% codeblock lang:swift%}
struct EvenNumberStruct {
    let number: UInt
    init?(number: UInt) {
        if number % 2 != 0 {
            return nil
        }
        self.number = number
    }   
}
{% endcodeblock %}

<!-- more -->

然而如把上面的结构换成类，就会得到下面的错误:

```
All stored properties of a class instance must be initialized before returning nil from an initializer
```

这是因为类的可失败构造函数只有在所有的类属性被初始化完成后才会触发失败行为，也就是说，在return nil之前，须保证所有的存储属性已被初始化（虽然我们的本意就是在这种情况下不必进行初始化）。在Swift 1.1中，可以将上边的存储属性改为可选类型，这样它就有一个默认的初始值nil。然而Swift 1.2中，改为UInt!并不会解决问题，事实上，只要把`self.number = number`放在`return nil`前面即可。

{% codeblock lang:swift%}
class EvenNumber {
    let number: UInt
    init?(number: UInt) {
        self.number = number // 在我测试的XCode 6.3.1中，这段代码必须在return nil之前
        if number % 2 != 0 {
            return nil
        }
    }
    private init(num: UInt) {
        self.number = num
    }
    class func defaultNumber() -> EvenNumber {
        return EvenNumber(num: 2)
    }
}
{% endcodeblock %}

需要注意的是，对于常量属性，也就是用let声明的属性，必须确保这个属性在return nil之前，在所有可能的执行路径中都被初始化。如下代码会报**variable 'self.number' used before initialized**
这样不知所云的错误

{% codeblock lang: swift%}
class EvenNumber {
    let number: UInt!
    init?(number: UInt) {
        if number > 1 {
            self.number = number
        }
        if number % 2 != 0 {
            return nil
        }
        
    }
    private init(num: UInt) {
        self.number = num
    }
    class func defaultNumber() -> EvenNumber {
        return EvenNumber(num: 2)
    }
}
{% endcodeblock %}

这可能跟Swift 1.2中的另一点改动有关：在一个构造函数中，常量属性只能被赋值一次，这意味着我们不再能在常量属性声明时赋予它一个初始值，然后在构造函数中覆盖它。（但同样由于Swift 1.2 的最新改动，我们可以先声明一个常量变量，只需保证在第一次使用它之前唯一的赋予它一个初始值即可。）把let改为**var**可解决上述问题，或者确保number在return nil前完全被初始化：

{% codeblock lang:swift%}
class EvenNumber {
    let number: UInt!
    init?(number: UInt) {
        if number > 0 {
            self.number = number
        } else {
            self.number = 0
        }
        if number % 2 != 0 {
            return nil
        }
        
    }
    private init(num: UInt) {
        self.number = num
    }
    class func defaultNumber() -> EvenNumber {
        return EvenNumber(num: 2)
   {% endcodeblock %}
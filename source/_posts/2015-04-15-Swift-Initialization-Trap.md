---
title: Swift的初始化限制导致的头痛问题
date: 2015-04-15 20:33:10
tags:
  - Swift
---

Swift在它的两步式构造过程中有严格的安全检查来保证构造的顺利进行，但这些初看上去有点晕的限制让我在初始化类自身的属性时费了不少劲。不是`Property 'self.xx' not initialized at super.init call`，就是`use of property 'xx' in base object before super.init initializes it`

先来复习一下Swift两步式[构造过程](http://numbbbbb.gitbooks.io/-the-swift-programming-language-/content/chapter2/14_Initialization.html)的规则:

> #### 阶段1
>
> - 某个指定构造器或便利构造器被调用；
> - 完成新实例内存的分配，但此时内存还没有被初始化；
> - 指定构造器确保其所在类引入的所有存储型属性都已赋初值。存储型属性所属的内存完成初始化；
> - 指定构造器将调用父类的构造器，完成父类属性的初始化；
> - 这个调用父类构造器的过程沿着构造器链一直往上执行，直到到达构造器链的最顶部；
> - 当到达了构造器链最顶部，且已确保所有实例包含的存储型属性都已经赋值，这个实例的内存被认为已经完全初始化。此时阶段1完成。
>
> #### 阶段2
>
> - 从顶部构造器链一直往下，每个构造器链中类的指定构造器都有机会进一步定制实例。构造器此时可以访问self、修改它的属性并调用实例方法等等。
> - 最终，任意构造器链中的便利构造器可以有机会定制实例和使用self。

<!-- more -->

也就是说，在调用父类构造器前，必须先完成自身属性的初始化：

{% codeblock lang:swift %}

class ViewController: UIViewController {
  private let animator: UIDynamicAnimator
  
  required init(coder aDecoder: NSCoder) {
    // ERROR: Property 'self.animator' not initialized at super.init call
    super.init(coder: aDecoder)
    animator = UIDynamicAnimator(referenceView: self.view)
  }
}

{% endcodeblock %}

然而，在父类的初始化过程完成前，也无法在子类中使用self.view，交换上面两行代码顺序后还是报错：

{% codeblock lang:swift %}

class ViewController: UIViewController {
  private let animator: UIDynamicAnimator
  
  required init(coder aDecoder: NSCoder) {
    // use of property 'view' in base object before super.init initializes it
    animator = UIDynamicAnimator(referenceView: self.view)
    super.init(coder: aDecoder)
  }
}

{% endcodeblock %}

在一些情况下，我们可以把初始化分为两步来解决问题，不过在这里`UIDynamicAnimator.referenceView`是个只读属性，并不能奏效：

{% codeblock lang:swift %}

class ViewController: UIViewController {
  private let animator: UIDynamicAnimator
  
  required init(coder aDecoder: NSCoder) {
    animator = UIDynamicAnimator() // 先不引用self
    super.init(coder: aDecoder)
    // ERROR: Cannot assign to the result of this expression
    animator.referenceView = self.view // 等父类构造过程完成
  }
}

{% endcodeblock %}

更好一点的方式，将这个属性改为`Optional`，并确保它在较早的阶段(构造完成后)被赋值(比如`viewDidLoad`)：

{% codeblock lang:swift %}

class ViewController: UIViewController {
  private var animator: UIDynamicAnimator! // 使用隐式解包可选属性可以避免以后每次使用时显示解包的麻烦
  
  required init(coder aDecoder: NSCoder) {
    super.init(coder: aDecoder)
  }
  
  override func viewDidLoad() {
    super.viewDidLoad()
    animator = UIDynamicAnimator(referenceView: self.view)
  }
}

{% endcodeblock %}


此外，还可以使用延迟存储属性和立即执行的闭包(不使用闭包依然无法在此时使用`self`)：

{% codeblock lang:swift %}

class ViewController: UIViewController {
  // 延迟属性必须用var声明
  lazy private var animator: UIDynamicAnimator = {
    return UIDynamicAnimator(referenceView: self.view)
  }()
  
  required init(coder aDecoder: NSCoder) {
    super.init(coder: aDecoder)
  }
  
  override func viewDidLoad() {
    super.viewDidLoad()
    animator.addBehavior(UIGravityBehavior())
  }
}

{% endcodeblock %}

这种方法的缺点是使用延迟属性的话就无法使用属性观察器了

我现在碰到的问题是，想在一个`UIView`的属性的`didSet`中调用`setNeedsDisplay()`，同时它的初始值构造时要用到`self.bounds`，因此无法使用延迟属性。使用可选值的话，又不知道如何在`UIView`中合理的给它一个初始值。烦。

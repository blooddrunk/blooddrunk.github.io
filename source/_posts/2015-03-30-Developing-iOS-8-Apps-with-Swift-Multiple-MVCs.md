---
title: 用Swift语言开发iOS8 App： Multiple MVC
date: 2015-03-30 23:19:49
tags:
  - Swift
  - iOS
---

iOS有一些内置Controller，它们的视图是另一个MVC：`UITabBarController`, `UISplitViewController`, `UINavigationController`, `UIPopoverController`

<!-- more -->

### UITabBarController

![UITabBarController](http://ww3.sinaimg.cn/mw690/6d325a28gw1eqpabtjcw4j20dw0okgnh.jpg)

`UITabBarController`绘制在屏幕下方绘制了最多5个tab标签(如果超过5个会自动呈现一个管理超出部分的界面)，上方呈现当前选定的tab的MVC。事实上，下方的tab栏的icon, title都是由`UITabBarController`包含的MVC的Controller的

{% codeblock lang:swift %}

var tabBarItem: UITabBarItem!

{% endcodeblock %}

属性所决定。tar bar item通常在*storyboard*中拖放进来。

### UISplitViewController

![UISplitViewController](http://ww4.sinaimg.cn/mw690/6d325a28gw1eqpabr9vblj210y0pm43e.jpg)

`UISplitViewController`只包括两个MVC：通常左侧*Master*决定了右侧的*Detail*的内容(每次都会生成一个新的Detail)。只有在iPad和iPhone6+的横屏模式可以同时显示Master和Detail，在iPad竖屏时只显示Detail，滑动屏幕才能看到Master。在其他尺寸的设备中需要嵌入`UINavigationController`来让UISplitViewController正常工作，这是由**Size Classes**决定的

### UINavigationController

![UINavigationController](http://ww3.sinaimg.cn/mw690/6d325a28gw1eqpablssn3j20du0oi40e.jpg)

`UINavigationController`大量运用于“设置”菜单中。可以把它想象成一个后进先出的Stack，最初的元素叫做`rootViewController`，最上面的元素叫做`visibleViewController`，它们都是`UINavigationController`的属性，每当在当前设置页面中点击一个子项，就相当于在上面push一个新项，而每点一次“后退”，就会把最上边的项pop掉(完全丢弃)。`UINavigationController`只负责呈现了上方的标题栏，下方的内容由Stack最上方的MVC所决定。甚至标题的内容，和标题栏的按钮也是由当前显示的MVC决定的(`navigationItem`属性)。

### 获得子MVC

`UITabBarController`, `UISplitViewController`, `UINavigationController`都一个数组属性表示它们包含的子MVC

{% codeblock lang:swift %}

var viewControllers: [UIViewController] { get set }

{% endcodeblock %}

相反的，每一个`UIViewController`也有三个对应的属性表示它是否在`UITabBarController`, `UISplitViewController`, `UINavigationController`中

{% codeblock lang:swift %}

var tabBarController: UITabBarController? { get }
var splitViewController: UISplitViewController? { get }
var navigationController: UINavigationController? { get }

//在Master中获得Detail
if let detailVC = splitViewController?.viewControllers[1] {
	...
}

{% endcodeblock %}

### Segue

创建好了子MVC后，我们还需要调用它们，这就是`Segue`，主要有以下几种`Segue`：

- Show Segue(如果在`UINavigationController`中，被调用的MVC就会被添加进`UINavigationController`的顶端，否则会展示为Modal窗口)
- Show Detail Segue(如果在`UISplitViewController`中，会展示一个Detail MVC，否则类似Show Segue)
- Modal Segue(展示为模态窗口，不太会用到)
- Popover Segue(展示为一个popover窗口，点击窗口以外的区域就会关闭该窗口)

**Segue每次都会创建一个新的MVC**，旧的会被抛弃

![Segue](http://ww4.sinaimg.cn/mw690/6d325a28gw1eqpabhneqyj209i080js9.jpg)

在storyboard中建立好Segue关系(通常是通过Control/右键拖动，比如在调用MVC中的一个按钮到目标MVC，点击这个按钮就会展示目标MVC)并填写好Segue的id后，往往还需要为目标MVC提供一定的初始化数据，为此在调用MVC中override`prepareForSegue`方法：

{% codeblock lang:swift %}

func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {
    if let identifier = segue.identifier {
        switch identifier {
            case “Show Graph”:
                if let vc = segue.destinationViewController as? CalcGraphController {
                    vc.property1 = ...
                    vc.callMethodToSetItUp(...)
                }
            default: break
        }
    }
}

{% endcodeblock %}

第一个参数`segue`有两个重要的属性，第一个是`identifier`(在storyboard的Attribules inspector中设置），用来标识调用了哪个Segue，另一个`destinationViewController`表示目标MVC，可利用它暴露的借口来做一些初始化工作，注意它是AnyObject，需要转换类型

需要注意的是，`prepareForSegue`执行的时候，`destinationViewController`中的outlets并没有被初始化，需要避免假设outlets有值而引发异常

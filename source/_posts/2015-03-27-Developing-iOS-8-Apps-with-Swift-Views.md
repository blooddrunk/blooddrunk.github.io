---
title: 用Swift语言开发iOS8 App： View
date: 2015-03-27 24:10:09
tags:
  - Swift
  - iOS
---

**Paul Hegary**的Swift[课程](https://itunes.apple.com/cn/course/developing-ios-8-apps-swift/id961180099 "Developing iOS 8 Apps with Swift")中关于**View**的一些笔记

1\. view(`UIView`的子类)代表了一块方形区域，定义的一块坐标空间用于绘制和处理事件


2\. 一个view只有一个父view

{% codeblock lang:swift %}

var superview: UIView?

{% endcodeblock %}

但可能有多个子view

{% codeblock lang:swift %}

var subviews: [AnyObject]    // 应转换为UIView类型

{% endcodeblock %}

`subviews`属性数组中位置靠后的view位于位置靠前的view的上面

<!-- more -->

3\. 以编程方式创建并添加view

{% codeblock lang:swift %}

superV.addSubView(aView: UIView)

subV.removeFromSuperView()

{% endcodeblock %}

4\. 在`UIViewController`中有一个属性，它表示了当前视图中最顶层的view，我们通常就是向这个顶级view中添加子view

{% codeblock lang:swift %}

var view: UIView

{% endcodeblock %}

5\. 坐标系统，`CGFloat` `CGFloat` `CGSize` `CGRect`

{% codeblock lang:swift %}

let cfg = CGFloat(aDouble)             // 坐标系统的单位总是CGFloat

var point = CGFloat(x: 37.0, y: 55.2)

var size = CGSize(width: 100.0, height: 50.0)

let rect = CGRect(origin: aCGPoint, size: aCGSize)

{% endcodeblock %}

坐标的原点位于左上角

坐标的单位是**point**，view的`contentScaleFactor`属性表示一个point有几个像素(通常是2)

view的`bounds`(`CGRect`)属性表示了它在自己的坐标系统的边界，因此`bounds`的`origin`默认为0, 0

view的`center`(`CGPoint`)属性表示了它的中心在**父view**中的坐标(使用`convertToPoint`获得`center`在当前view的坐标)

类似的，`frame`(`CGRect`)属性表示它在**父view**坐标系统中的矩形边界

6\. 自定义view：新建一个UIView的子类并 override`drawRect`方法。

> **不要**直接调用`drawRect`，使用`setNeedsDisplay`来间接调用它以实现重绘

实现`drawRect`：

{% codeblock lang:swift %}

let path = UIBezierPath()

path.moveToPoint(CGPoint(80, 50)) // assume screen is 160x250
path.addLineToPoint(CGPoint(140, 150))
path.addLineToPoint(CGPoint(10, 150))

path.closePath()

UIColor.greenColor().setFill() // note this is a method in UIColor, not UIBezierPath
UIColor.redColor().setStroke() // note this is a method in UIColor, not UIBezierPath
path.linewidth = 3.0 // note this is a property in UIBezierPath, not UIColor

{% endcodeblock %}

7\. 字体。通常使用以下类静态方法获取不同类型文本的字体，而不是指定固定的大小：

{% codeblock lang:swift %}

class func preferredFontForTextStyle(UIFontTextStyle) -> UIFont

// 部分可用的style
UIFontTextStyle.Headline
UIFontTextStyle.Body
UIFontTextStyle.Footnote

// 指定大小的系统字体，不要在用户内容中使用它们
class func systemFontOfSize(pointSize: CGFloat) -> UIFont
class func boldSystemFontOfSize(pointSize: CGFloat) -> UIFont

{% endcodeblock %}

8\. `bounds`改变时view的行为

{% codeblock lang:swift %}

var contentMode: UIViewContentMode

{% endcodeblock %}

  * 不缩放view，只移动到某个位置
 `.Left/.Right/.Top/.Bottom/.TopRight/.TopLeft/.BottomRight/.BottomLeft/.Center`
  * 缩放
 `.ScaleToFill/.ScaleAspectFill/.ScaleAspectFit`，`.ScaleToFill`是默认选项
  * 再次调用`drawRect`，重绘
 `.Redraw`

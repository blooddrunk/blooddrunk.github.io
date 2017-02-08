---
title: Swift中的NSNotification
date: 2015-04-20 16:15:18
tags:
  - Swift
  - iOS
---

`NSNotification`提供了在应用程序中广播和接受消息的方法。

### 注册一个通知

注册一个通知非常简单，获取默认的“通知中心”，然后调用`addObserverForName`方法

![addObserverForName](http://ww3.sinaimg.cn/mw690/6d325a28gw1erc345st0uj20pc0g8gs7.jpg)

<!-- more -->

比如，监视用户在设置中改变字体的通知：

{% codeblock lang:swift %}

let center = NSNotificationCenter.defaultCenter() // 获取用于系统通知的通知中心

center.addObserverForName(UIContentSizeCategoryDidChangeNotification // 要监视的通知的名称
                    object: UIApplication.sharedApplication(), // 要见识的通知发送者(nil表示所有发送者)
                    queue: NSOperationQueue.mainQueue()) // 用于执行接收到通知后要执行的闭包的队列
{ notification in // 这其实是个trailing闭包，是第四个参数
	// ...
    let c = notification.userInfo?[UIContentSizeCategoryNewValueKey] // 获取设置的新的字体，比如UIContentSizeCategorySmall
	// userInfo是随通知发送的包含相关信息的字典
}

{% endcodeblock %}

### 创建一个通知

除了监听系统通知，我们自己也可以创建通知

![NSNotification](http://ww3.sinaimg.cn/mw690/6d325a28gw1erc3wr1koej20ou0aagqu.jpg)

{% codeblock lang:swift %}

let center = NSNotificationCenter.defaultCenter() // 获取用于系统通知的通知中心

let notification = NSNotification(name: GPXURL.Notification, object: self, userInfo: [GPXURL.Key:url]) // 创建一个通知

center.postNotification(notification) // 发送通知

{% endcodeblock %}

然后我们就可以根据创建`NSNotification`是提供的参数(`name`和`object`)来监视这个通知了，并可以取得创建时提供的`userInfo`字典
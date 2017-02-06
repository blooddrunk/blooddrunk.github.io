---
title: iOS应用程序生命周期
date: 2015-04-27 20:09:26
tags:
  - Swift
  - iOS
---

iOS app主要有五个状态，**Not running**, **Inactive**, **Active**, **Background**, **Suspended**。它们之间的转换关系如下：

![States Changes in an iOS app](http://ww2.sinaimg.cn/mw690/6d325a28jw1erk66ylphkj20le0oegnc.jpg)

| 状态 | 说明 |
|------|------|
| Not running | app尚未被启动或者被用户或系统终止 |
| Inactive | app正在前台运行但是尚未开始接受UI事件（但它可能在执行其他代码）。一个app通常只在这个状态停留一段短暂的时间，然后转换到下个状态。在进入inactive状态时，由于马上就会过渡到background或active状态，app此时应该让自己保持不活动的状态。 |
| Active | app正在前台运行并在接受事件。这就是我们通常看到的前台程序的状态。一个处于active状态的app没有特殊的限制，它应当对用户操作做出响应。 |
| Background | app正在执行代码但没有显示在屏幕上。当用户退出一个app时，系统在中止这个app之前（进入suspended状态）使它短暂的置于background状态。在其他情况下，系统可能在后台启动某个app（或者唤醒一个处于suspended状态的app），给它一定时间来处理某些特定的任务。比如，系统可能唤醒一个app让它来进行后台下载，处理和位置的有关的事件，远程通知或其他类型的事件。处于background状态的app应当做的工作越少越好，它应该尽快把控制还给系统。 |
| Suspended | app还在内存中但没有在执行任何代码。系统会中止那些没有任何挂起的任务要完成的后台程序。系统可能在任何时候清理这些程序来为其他app提供空间。 |

启动某个app：not running -> inactive -> active

从一个app切换到另一个app：active -> inactive -> background -> suspended

<!-- more -->

### AppDelegate.swift

Xcode自动提供的`AppDelegate`类（实现了`UIApplicationDelegate`接口）包含这个app的启动代码和app生命周期的重要状态变化。简单介绍一下状态发生变化会被调用的重要方法：

#### Not running -> Inactive

{% codeblock lang:swift %}
optional func application(_ application: UIApplication,
	didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool
{% endcodeblock %}

> `launchOptions`参数是一个包括了app被启动原因的字典（直接被用户启动的情况下可能为空）

> 也可以监视`UIApplicationDidFinishLaunchingNotification`系统通知实现相同的功能。

#### Active -> Inactive

{% codeblock lang:swift %}
optional func applicationWillResignActive(_ application: UIApplication)
{% endcodeblock %}

> 可以在这里暂停UI活动

#### Inactive -> Active

{% codeblock lang:swift %}
optional func applicationDidBecomeActive(_ application: UIApplication)
{% endcodeblock %}

> 也可以监视`UIApplicationWillResignActiveNotification`通知

#### Inactive -> Background

{% codeblock lang:swift %}
optional func applicationDidEnterBackground(_ application: UIApplication)
{% endcodeblock %}

在这个方法中释放共享资源，保存当前的应用程序状态（因为稍后它有可能被系统终止）

> 这个方法只有大概5秒的时间来执行相关任务。如果需要额外的时间来执行一些重要的清理的工作，调用`beginBackgroundTaskWithExpirationHandler`（比如将一些重要的信息传送到远程主机上），但它不应当用来简单的延长程序在后台的执行。通常在`applicationDidEnterBackground`中执行的任务越少越好。

> 也可以监视`UIApplicationDidEnterBackgroundNotification`通知

#### Background -> Inactive

{% codeblock lang:swift %}
optional func applicationWillEnterForeground(_ application: UIApplication)
{% endcodeblock %}

app从后台重新回到前台，可以在这里撤销在app进入Background时做的一些工作。

> 也可以监视`UIApplicationWillEnterForegroundNotification`通知
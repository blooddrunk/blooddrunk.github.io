---
title: (面试传家宝)在浏览器中输入一个URL之后发生了什么
tags:
  - 网络
categories:
  - 前端
  - 面试
date: 2015-12-08 23:27:38
---

> http://stackoverflow.com/questions/2092527/what-happens-when-you-type-in-a-url-in-browser#

> https://github.com/markyun/My-blog/tree/master/Front-end-Developer-Questions/Questions-and-Answers

详细版：
  1. 浏览器会开启一个线程来处理这个请求，对 URL 分析判断如果是 http 协议就按照 Web 方式来处理;
  2. 调用浏览器内核中的对应方法，比如 WebView 中的 loadUrl 方法;
  3. 通过DNS解析获取网址的IP地址，设置 UA 等信息发出第二个GET请求;
  4. 进行HTTP协议会话，客户端发送报头(请求报头);
  5. 进入到web服务器上的 Web Server，如 Apache、Tomcat、Node.JS 等服务器;
  6. 进入部署好的后端应用，如 PHP、Java、JavaScript、Python 等，找到对应的请求处理;
  7. 处理结束回馈报头，此处如果浏览器访问过，缓存上有对应资源，会与服务器最后修改时间对比，一致则返回304;
  8. 浏览器开始下载html文档(响应报头，状态码200)，同时使用缓存;
  9. 文档树建立，根据标记请求所需指定MIME类型的文件（比如css、js）,同时设置了cookie;
  10. 页面开始渲染DOM，JS根据DOM API操作DOM,执行事件绑定等，页面显示完成。

简洁版：
  1. 浏览器根据请求的URL交给DNS域名解析，找到真实IP，向服务器发起请求；
  2. 服务器交给后台处理完成后返回数据，浏览器接收文件（HTML、JS、CSS、图象等）；
  3. 浏览器对加载到的资源（HTML、JS、CSS等）进行语法解析，建立相应的内部数据结构（如HTML的DOM）；
  4. 载入解析到的资源文件，渲染页面，完成。
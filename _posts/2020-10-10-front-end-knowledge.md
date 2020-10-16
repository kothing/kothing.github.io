---
layout: post
title:  "Front-end knowledge"
author: Kothing
categories: [ Web ]
tags: [Javascript]
image: assets/images/11.jpg
description: "Front-end knowledge"
rating: 5
---

Front-end knowledge

### 大纲

1. 浏览器篇 
   - 1.1. 常用那几种浏览器测试？主流浏览器的内核有哪些？ 
   - 1.2. 一个页面从输入 URL 到页面加载显示完成，这个过程中都发生了什么？ 
   - 1.3. 浏览器缓存 
   - 1.4. HTTP 
      - 1.5.1. http状态码 
      - 1.5.2. URL和URI有什么区别？ 
      - 1.5.3. HTTP和HTTPS的区别 
2. HTML篇 
   - 2.1. Doctype作用？标准模式与兼容模式各有什么区别? 你知道多少种Doctype文档类型 
   - 2.2. 说说你对语义化的理解？ 
   - 2.3. HTML与XHTML有什么区别? 
   - 2.4. 页面导入样式时，使用link和@import有什么区别？ 
   - 2.5. HTML5有哪些新特性？ 
   - 2.6. iframe的优缺点？ 
   - 2.7. img中的alt与title属性 
   - 2.8. HTML 的中，如何写一个值为 “a”=‘b’ 的属性值？ 
3. CSS篇 
   - 3.1. 行内元素有哪些？块级元素有哪些？CSS的盒模型？ 
   - 3.2. 清除浮动有哪些方式？ 
   - 3.3. CSS选择符都有哪些？哪些属性可以继承？优先级算法如何计算？ 
   - 3.4. CSS3新增伪类有哪些？ 
   - 3.5. 如何居中div？ 
   - 3.6. 为什么要初始化CSS？ 
   - 3.7. 说说你对BFC规范的理解？ 
   - 3.8. 讲讲 position float display 各有哪些取值，它们互相之间会如何影响？ 
   - 3.9. 纯CSS实现三角形 
4. JS篇 
   - 4.1. 面向对象 
   - 4.2. 什么是闭包？闭包的特性是什么？ 
   - 4.3. 继承 
   - 4.4. DOM操作（添加、移除、移动、复制、创建和查找节点） 
   - 4.5. 介绍一下new 操作符。New操作符具体干了什么？ 
   - 4.6. HTML5离线存储 
   - 4.7. JS的数据类型 
   - 4.8. null和undefined的区别 
   - 4.9. call()和apply()的区别 
   - 4.10. 深拷贝和浅拷贝 
   - 4.11. ajax 
   - 4.12. 数组去重 
   - 4.13. this对象 
   - 4.14. eval() 
   - 4.15. 什么是UA？ 
   - 4.16. 什么是事件委托？ 
   - 4.17. promise 
   - 4.18. window.onload和document.ready的区别？哪一个先执行？ 
   - 4.19. var、let和const有什么区别 
   - 4.20. JavaScript 启动后，内存中有多少个对象？如何用代码来获得这些信息？ 
5. 前端安全问题 
   - 5.1. XSS 
   - 5.2. CSRF 
6. 跨域 
   - 6.1. 什么是跨域？ 
   - 6.2. 什么是同源策略？ 
   - 6.3. 解决跨域的方法有哪些？ 
7. 性能优化 
   - 7.1. 性能优化有哪些方法？ 
   
   
## 浏览器篇 
### 1.1. 常用那几种浏览器测试？主流浏览器的内核有哪些？
IE、Safari、Chrome、Mozilla Firefox、Opera

+ 1、Trident内核 
代表产品为Internet Explorer，又称其为IE内核。Trident（又称为MSHTML），是微软开发的一种排版引擎 。
+ 2、Gecko内核 
代表作品为Mozilla Firefox。Gecko是一套开放源代码的、以C++编写的网页排版引擎，是最流行的排版引擎之一，仅次于Trident。使用它的最著名浏览器有Firefox。
+ 3、WebKit内核 
代表作品有Safari、Chrome。WebKit是一个开源项目，主要用于Mac OS系统，它的特点在于源码结构清晰、渲染速度极快。缺点是对网页代码的兼容性不高，导致一些编写不标准的网页无法正常显示。
+ 4、Presto内核 
代表作品Opera。Presto是由Opera Software开发的浏览器排版引擎，供Opera 7.0及以上使用。

### 1.2 一个页面从输入 URL 到页面加载显示完成，这个过程中都发生了什么？
1. 浏览器根据请求的URL交给DNS域名解析，找到真实IP
2. 浏览器根据 IP 地址向服务器发起 TCP 连接，与浏览器建立 TCP 三次握手
   - a.客户端向服务器发送一个建立连接的请求
   - b.服务器接到请求后发送同意连接的信号
   - c.客户端接到同意连接的信号后，再次向服务器发送了确认信号，然后客户端与服务器的连接建立成功
3. 浏览器发送HTTP请求
浏览器根据 URL 内容生成 HTTP 请求，请求中包含请求文件的位置、请求文件的方式等等；
4. 服务器处理请求并返回HTTP报文（HTTP响应报文也是由三部分组成: 状态码, 响应报头和响应报文）：
   - a. 服务器接到请求后，会根据 HTTP 请求中的内容来决定如何获取相应的 HTML 文件；
   - b. 服务器将得到的 HTML 文件发送给浏览器；
   - c. 在浏览器还没有完全接收 HTML 文件时便开始渲染、显示网页；
   - d. 在执行 HTML 中代码时，根据需要，浏览器会继续请求图片、CSS、JavsScript等文件，过程同请求 HTML 。
5. 断开连接

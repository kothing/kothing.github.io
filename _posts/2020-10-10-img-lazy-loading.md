---
layout: post
title:  "网站开发中，如何实现图片的懒加载"
author: Kothing
categories: [ Web ]
tags: [Web, Javascript]
image: assets/images/11.jpg
description: "网站开发中，如何实现图片的懒加载"
rating: 4.5
---

懒加载，顾名思义，在当前网页，滑动页面到能看到图片的时候再加载图片

故问题拆分成两个：

如何判断图片出现在了当前视口 （即如何判断我们能够看到图片）
如何控制图片的加载

## 方案一
### 如何判断图片出现在了当前视口
`clientTop`，`offsetTop`，`clientHeight` 以及 `scrollTop` 各种关于图片的高度作比对  

这些高度都代表了什么意思？  

这我以前有可能是知道的，那时候我比较单纯，喜欢死磕。我现在想通了，背不过的东西就不要背了  

所以它有一个问题：复杂琐碎不好理解！  

仅仅知道它静态的高度还不够，我们还需要知道动态的  

如何动态？监听 `window.scroll` 事件  

### 如何控制图片的加载
```html
<img data-src="shanyue.jpg">
```
首先设置一个临时属性 `data-src`，控制加载时使用 `src` 代替 `data-src`


## 方案二
如何判断图片出现在了当前视口
引入一个新的 API， `Element.getBoundingClientRect()` 方法返回元素的大小及其相对于视口的位置。
![getBoundingClientRect()](https://mdn.mozillademos.org/files/15087/rect.png 'getBoundingClientRect()')  



## 方案三


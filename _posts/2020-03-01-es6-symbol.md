---
layout: post
title:  "深度理解和使用ES6中的Symbol"
author: Kothing
categories: [ Jekyll, tutorial ]
image: assets/images/17.jpg
rating: 4
---
ES6中引入了一种新的基础数据类型：`Symbol`，不过很多开发者可能都不怎么了解它，或者觉得在实际的开发工作中并没有什么场景应用到它，那么今天我们来讲讲这个数据类型，并看看我们怎么来利用它来改进一下我们的代码。

**新的基础数据类型（primitive type）**

`Symbol`是由ES6规范引入的一项新特性，它的功能类似于一种标识唯一性的ID。通常情况下，我们可以通过调用`Symbol()`函数来创建一个Symbol实例：

```
let s1 = Symbol()
```

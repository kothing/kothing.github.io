---
layout: post
title: "关于前端框架MVC和MVVM以及前端工程化的理解"
author: Kothing
categories: [ Javascript ]
image: assets/images/13.jpg
rating: 5
hidden: true
---

## 一、概念解释
MVVM 是`Model-View-ViewModel` 的缩写，它是一种基于前端开发的架构模式，其核心是提供对`View` 和 `ViewModel` 的双向数据绑定，这使得`ViewModel` 的状态改变可以自动传递给 `View`，即所谓的数据双向绑定。

以Vue.js 为例。Vue是一个提供了 `MVVM` 风格的双向数据绑定的 Javascript 库，专注于View 层。它的核心是 `MVVM` 中的 `VM`，也就是 `ViewModel`。 `ViewModel`负责连接 `View` 和 `Model`，保证视图和数据的一致性，这种轻量级的架构让前端开发更加高效、便捷。

## 二、关于 MVC
`MVC` 即 `Model-View-Controller` 的缩写，就是 模型—视图—控制器，也就是说一个标准的Web 应用程式是由这三部分组成的：  

**View** ：用来把数据以某种方式呈现给用户  

**Model** ：其实就是数据  

**Controller** ：接收并处理来自用户的请求，并将 Model 返回给用户  

在HTML5 还未火起来的那些年，MVC 作为Web 应用的最佳实践是OK 的，这是因为 Web 应用的View 层相对来说比较简单，前端所需要的数据在后端基本上都可以处理好，View 层主要是做一下展示，那时候提倡的是 Controller 来处理复杂的业务逻辑，所以View 层相对来说比较轻量，就是所谓的瘦客户端思想。  

## 三、前端要工程化
相对 HTML4，HTML5 最大的亮点是它为移动设备提供了一些非常有用的功能，使得 HTML5 具备了开发App的能力， HTML5开发App 最大的好处就是跨平台、快速迭代和上线，节省人力成本和提高效率，因此很多企业开始对传统的App进行改造，逐渐用H5代替Native。使用H5 来构建 App， 那View 层所做的事，就不仅仅是简单的数据展示了，它不仅要管理复杂的数据状态，还要处理移动设备上各种操作行为等等。因此，前端也需要工程化，也需要一个类似于MVC 的框架来管理这些复杂的逻辑，使开发更加高效。 
但这里的 MVC 又稍微发了点变化： 

**View** ：UI布局，展示数据  

**Model**：管理数据  

**Controller** ：响应用户操作，并将 Model 更新到 View 上  

这种 MVC 架构模式对于简单的应用来看是OK 的，也符合软件架构的分层思想。 但实际上，随着H5 的不断发展，人们更希望使用H5 开发的应用能和Native 媲美，或者接近于原生App 的体验效果，于是前端应用的复杂程度已不同往日，今非昔比。这时前端开发就暴露出了三个痛点问题：

1. 开发者在代码中大量调用相同的 DOM API，处理繁琐 ，操作冗余，使得代码难以维护。
2. 大量的DOM 操作使页面渲染性能降低，加载速度变慢，影响用户体验。
3. 当 Model 频繁发生变化，开发者需要主动更新到View ；当用户的操作导致 Model 发生变化，开发者同样需要将变化的数据同步到Model 中，这样的工作不仅繁琐，而且很难维护复杂多变的数据状态。

其实，早期jquery的出现就是为了前端能更简洁的操作DOM 而设计的，但它只解决了第一个问题，另外两个问题始终伴随着前端一直存在。  

`MVVM` 的出现，完美解决了以上三个问题。  

`MVVM` 由 `Model`、`View`、`ViewModel` 三部分构成，`Model` 层代表数据模型，也可以在Model中定义数据修改和操作的业务逻辑；`View` 代表UI 组件，它负责将数据模型转化成UI 展现出来，`ViewModel` 是一个同步`View` 和 `Model`的对象。  

在`MVVM`架构下，`View` 和 `Model` 之间并没有直接的联系，而是通过`ViewModel`进行交互，`Model` 和 `ViewModel` 之间的交互是双向的， 因此`View`数据的变化会同步到`Model`中，而`Model` 数据的变化也会立即反应到`View` 上。  

`ViewModel` 通过双向数据绑定把 `View` 层和 `Model` 层连接了起来，而`View` 和 `Model` 之间的同步工作完全是自动的，无需人为干涉，因此开发者只需关注业务逻辑，不需要手动操作DOM， 不需要关注数据状态的同步问题，复杂的数据状态维护完全由 `MVVM` 来统一管理。

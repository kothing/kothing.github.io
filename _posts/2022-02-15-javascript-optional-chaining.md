---
layout: post
title: "Javascript可选链操作符(?.)"
author: Kothing
categories: [ Javascript ]
image: assets/images/11.jpg
featured: false
rating: 5
---

# 可选链操作符
可选链（Optional chaining） `?.` 是一种以安全的方式去访问嵌套的对象属性，即使某个属性根本就不存在。
这是一项新的提案，老旧浏览器可能需要 polyfills。

## 一、解决的问题：
### 1、问题一
如果用户信息中，地址是非必填的，那我们就无法安全地访问地址的某一个属性：
```js
let user = {}; // 用户可能没有填地址
alert(user.address.street); // 报错
```

### 2、问题二
获取一个 DOM 元素，但这个 DOM 元素可能不存在：
```js
// 当 querySelector(...) 的结果为 null 的时候，程序会报错
let html = document.querySelector('.my-element').innerHTML;
```
在可选链出现前，我们一般通过逻辑与操作来解决：
```js
let user = {}; // 没有地址的用户
alert(user && user.address && user.address.street); // undefined (不会报错)
```
使用逻辑与操作符可以确保表达式的所有部分都能够正确执行，但写法却比较笨重。

## 二、基础用法
可选链 `?.` 能够使代码变得简便，当位于 `?.` 前面的值为 `undefined` 或 `null` 时，会立即阻止代码的执行，并返回 `undefined`。

通过可选链，我们可以安全地访问用户的地址：
```js
let user = {}; // 一个没有地址的用户
alert(user?.address?.street); // undefined (不会报错)
```
即使 user 对象不存在，使用可选链访问它的地址属性也不会报错：
```js
let user = null;
alert(user?.address); // undefined
alert(user?.address.street); // undefined
```
**注意**
不要过度使用可选链，一般只在希望某个值可能不存在的情况下，才使用 `?.`例如，根据我们的代码逻辑，`user` 对象必须存在，但 `address` 属性是可选的，所以 `user.address?.street` 才是更好的选择。这样，由于其他原因导致的 `user` 对象为 `undefined` 的情况才能被快速发现。
位于 `?.` 前的变量必须被显示声明，如果 `user` 这个变量根本没有被声明，那么 `user?.anything` 将会触发一个错误：
```js
// ReferenceError: user is not defined
user?.address;
```
此处必须有变量声明语句 let/const/var， 可选链对未声明的变量无效

## 三、其它用法
### 1、短路
上面说到，在可选链中 `?.` 前面的部分值为 `null` 或 `undefined` 时，会立即停止执行。
所以，如果在其后面如果有函数的调用，或者其他操作，都不会执行。
```js
let user = null;
let x = 0;
user?.sayHi(x++); // 什么都不会做
alert(x);   // 0，值没有自增
```

### 2、?.()
`?.()` 可用于执行一个可能不存在的函数。
下面的代码中，有些用户拥有 admin 方法，有些用户没有：
```js
let user1 = {
  admin() {
    alert("I am admin");
  }
};

let user2 = {};

user1.admin?.(); // I am admin
user2.admin?.(); // 啥都没有（因为没有这样的方法）
```
这里首先使用点符号（userAdmin.admin）来获取 `admin` 方法，因为 `user` 对象一定存在，所以可以安全地读取。

然后 `?.()` 会检查它左边的部分：如果 admin 函数存在，那么就调用运行它（user1）。否则运算停止且不报错（user2）。

### 3、?.[]
`?.` 语法同样可以在需要用中括号去访问属性时使用，使用它可以安全的访问一个或许还不存在的对象的属性：
`js``
let user1 = {
  firstName: "John"
};

let user2 = null; // 假设，我们不能授权此用户

let key = "firstName";

alert(user1?.[key]); // John
alert(user2?.[key]); // undefined

alert(user1?.[key]?.something?.not?.existing); // undefined
```

`?.` 可以与 `delete` 操作符共用
```js
delete user?.name; // 如果 user 存在，则删除 user.name
```
需要注意的是，使用 ?. 可以进行删除和读取操作，但是不能进行赋值操作
```js
let user = null;

user?.name = "John"; // Error，不起作用
// 因为它在计算的是 undefined = "John"
```

## 四、总结
可选链 ?. 有三种形式：

+ obj?.prop —— 如果 `obj` 存在则返回 `obj.prop`，否则返回 `undefined`。
+ obj?.[prop] —— 如果 `obj` 存在则返回 `obj[prop]`，否则返回 `undefined`。
+ obj.method?.() —— 如果 `obj.method` 存在则调用 `obj.method()`，否则返回 `undefined`。
这几种形式都是检查 `?.` 左侧的值是否为 `null` 或 `undefined`，如果不是的话则继续执行。

**注意**：应该仅在 `?.` 左侧的值可能不存在的情况下才使用，这样发生错误时才能更容易地找到问题。


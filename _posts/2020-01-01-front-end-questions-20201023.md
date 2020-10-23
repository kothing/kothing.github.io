---
layout: post
title:  "前端经典题目第一期"
author: Kothing
categories: [ Questions ]
tags: [Web,JavaScript]
image: assets/images/13.jpg
description: "前端经典题目第一期"
rating: 4.5
---
前端经典题目第一期

## 打印一个乘法表
用js打印一个乘法表
```js
console.log(`
1*1=1
2*1=2 2*2=4
3*1=3 3*2=6 3*3=9
4*1=4 4*2=8 4*3=12 4*4=16
5*1=5 5*2=10 5*3=15 5*4=20 5*5=25
6*1=6 6*2=12 6*3=18 6*4=24 6*5=30 6*6=36
7*1=7 7*2=14 7*3=21 7*4=28 7*5=35 7*6=42 7*7=49
8*1=8 8*2=16 8*3=24 8*4=32 8*5=40 8*6=48 8*7=56 8*8=64
9*1=9 9*2=18 9*3=27 9*4=36 9*5=45 9*6=54 9*7=63 9*8=72 9*9=81
`)
```
代码
```js
let str = '';
for (let i = 1; i < 10; i += 1) {
  for (let j = 1; j <= i; j += 1) {
    str += i + '*' + j + '=' + i * j + '\t';
  }
  str += '\n';
}
console.log(str);
```

## 斐波那契数列函数
斐波那契数，指的是这样一个数列：1、1、2、3、5、8、13、21、……在数学上，斐波那契数列以如下被以递归的方法定义：F0=0，F1=1，Fn=Fn-1+Fn-2(n>=2，n∈N*)，用文字来说，就是斐波那契数列由 0 和 1 开始，之后的斐波那契数列系数就由之前的两数相加。　　

常用的计算斐波那契数列的方法分为两大类：递归和循环。

### 递归
**方法一：普通递归**
代码优美逻辑清晰。但是有重复计算的问题，如：当n为5的时候要计算fibonacci(4) + fibonacci(3)，当n为4的要计算fibonacci(3) + fibonacci(2) ，这时fibonacci(3)就是重复计算了。运行 fibonacci(50) 会出现浏览器假死现象，毕竟递归需要堆栈，数字过大内存不够。
```js
function fibonacci(n) {
    if (n == 1 || n == 2) {
        return 1;
    };
    return fibonacci(n - 2) + fibonacci(n - 1);
}
fibonacci(30);
```

**方法二：改进递归-把前两位数字做成参数避免重复计算**
```js
function fibonacci(n) {
    function fib(n, v1, v2) {
        if (n == 1)
            return v1;
        if (n == 2)
            return v2;
        else
            return fib(n - 1, v2, v1 + v2)
    }
    return fib(n, 1, 1)
}
fibonacci(30)
```

**方法三：改进递归-利用闭包特性把运算结果存储在数组里，避免重复计算**
```js
var fibonacci = function () {
    let memo = [0, 1];
    let fib = function (n) {
        if (memo[n] == undefined) {
            memo[n] = fib(n - 2) + fib(n - 1)
        }
        return memo[n]
    }
    return fib;
}()
fibonacci(30)
```

**方法三1：改进递归-摘出存储计算结果的功能函数**
```js
var memoizer = function (func) {
    let memo = [];
    return function (n) {
        if (memo[n] == undefined) {
            memo[n] = func(n)
        }
        return memo[n]
    }
};
var fibonacci=memoizer(function(n){
    if (n == 1 || n == 2) {
        return 1
    };
    return fibonacci(n - 2) + fibonacci(n - 1);
})
fibonacci(30)
```

### 循环
**方法一：普通for循环** 
```js
function fibonacci(n) {
    var n1 = 1, n2 = 1, sum;
    for (let i = 2; i < n; i++) {
        sum = n1 + n2
        n1 = n2
        n2 = sum
    }
    return sum
}
fibonacci(30)
```

**方法二：for循环+解构赋值** 
```js
var fibonacci = function (n) {
    let n1 = 1; n2 = 1;
    for (let i = 2; i < n; i++) {
        [n1, n2] = [n2, n1 + n2]
    }
    return n2
}
fibonacci(30)
```

## 手写实现Promise
Promise是ES6中提供的一种异步编程的解决方案，是一种强大而好用的方法，为解决回调地狱提供了优美的解决办法。
Promise 的基本用法：
Promise构造函数接受一个函数作为参数，此函数传入两个参数resolve函数和reject函数，分别作为执行成功或者执行失败的函数
```js
const promiseFn = new Promise((resolve, reject) => {
  if ('异步执行成功') {
    resolve('需要返回的值');
  } else {
    reject('错误信息');
  }
});
// new Promise()创建的promiseFn是一个异步函数
```
上文通过new Promise() 创建的promiseFn可以通过then执行成功后的代码，同时then接受两个函数作为参数，第二个参数是可选的
```js
promiseFn().then(() => {
  // 成功后的操作
}, () => {
  // 失败后的操作
});
```

知道了 Promise 的基本用法，那么我们就来模拟一下它
第一步，当然是要一个构造函数了，为了语义化，我就给它命名为  `_Promise`
```js
function _Promise(resolver) {
  this._status = 'pending';
  this._result = '';
  resolver(this.resolve.bind(this), this.reject.bind(this));
}
```

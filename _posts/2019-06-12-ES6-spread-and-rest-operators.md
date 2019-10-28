---
layout: post
title:  "ES6扩展操作符和剩余操作符..."
author: Kothing
categories: [ javascript, ES6 ]
image: assets/images/16.jpg
rating: 4.5
---
扩展运算符写法是三个点...，写法虽然跟剩余操作符一致，都是...，但是作用可以认为是相反的。

## 扩展运算符

扩展运算符的核心就是2个字：打散。

剩余操作符的核心就是2个字：打包。

### 剩余操作符和扩展运算符在赋值方面的对比：
剩余操作符是：表示剩下的**打包**，通常是只可能放在**变量名**前面；一定有赋值或者传值操作：
```js
let [a, ...b] = [1,2,3,4,5];
b; // [2,3,4,5]
```
扩展运算符是：表示把打包好的**打散**，可能放在**变量名**前面，也可能放在**值**的前面；可能有赋值操作，也可能没有赋值操作：
```js
let a = [1, ...[2,3,4]]; // 注意，既然是先打散然后赋值给一个变量，就需要用[]包起来形成数组
a; // [1, 2, 3, 4]
```
```js
console.log(1, ...[2,3,4]); // 1, 2, 3, 4
```
### 剩余操作符跟扩展运算符的在函数方面的应用对比：

+ 剩余操作符举例：

```js
function ff (...a) {
    return a[0] + '-' + a[1] + '-' + a[2];
}

ff(1,3,5); // '1-3-5'
```

+ 扩展运算符举例：

```js
function ff (a,b,c) {
    return a + '-' + b + '-' + c;
}

let x = [1,3,5];

ff(...x); // '1-3-5'
```

扩展运算符可以跟表达式，将表达式的运算结果打散。
```js
console.log(...('abcd'.split(''))); // a, b, c, d
```

### 本质理解
其实ES6之所以把这两个关键字设成一致的写法，是因为它们本质是一样，都是解构赋值。

我们复习一下解构赋值，下面用的是剩余操作符：
```js
let [a, b, ...c] = [1,2,3,4,5];
```
JS引擎会拿元素依次为变量赋值，到c变量的时候，会一次性的把[3,4,5]赋值给c，虽然直观的感受是c变量打包了3个元素，但是其实等于是...把c打散成了3个变量位置，然后一对一赋值，最后再合成一个数组。

再看运用扩展运算符的解构赋值：
```js
let [a, b, c] = [...[1,2,3]];
c; // 3
```
其实...也是先打散数组，然后再一对一赋值。

最后看一个朴素的解构赋值的栗子：
```js
let [...a] = [...[1,2,3]]; // 右边打散成元素，左边又给打包起来
a; // [1, 2, 3]

// 忽略掉外面的数组括号，忽略掉三个点，就相当于：
let a = [1,2,3];
```

### 扩展运算符的应用（核心就是打散）
**一、把数组打散成多个参数**

经常看js奇技淫巧的同学，应该知道把数组打散成多个参数，有一个方法是用apply方法，比如，找出数组里的最大的数：
```js
Math.max.apply(null, [14, 3, 77]); // 77
```
这种写法利用的apply的特性：foo.apply(this,arguments)==this.foo(arg1, arg2, arg3)，由于apply方法比较难理解，一般使用者也比较抵触用这个方法，所以应用不多，现在好了，有了扩展运算符，ES算是给了一个官方的打散数组为参数的方法。
```js
Math.max(...[14, 3, 77])
```

**二、一次性合并多个数组**

ES5确实已经有很好的合并方法.concat，所以扩展运算符只是提供了另一条路，ES6这个扩展运算符写法的好处是不借助函数，只是JS运算。
```js
// ES5
[1, 2].concat(more)
// ES6
[1, 2, ...more] // ...会把more打散
```
```js
var arr1 = ['a', 'b'];
var arr2 = ['c'];
var arr3 = ['d', 'e'];

// ES5的合并数组
arr1.concat(arr2, arr3);
// [ 'a', 'b', 'c', 'd', 'e' ]

// ES6的合并数组
[...arr1, ...arr2, ...arr3]
// [ 'a', 'b', 'c', 'd', 'e' ]
```

**三、复制数组**

由于数组赋值是传址而不是传值，所以ES5复制数组的方法是这样：
```js
const a1 = [1, 2];
const a2 = a1.concat(); // concat会生成新的数组
```
现在ES6可以这么写：
```js
const a1 = [1, 2];
// 写法一，利用扩展运算符
const a2 = [...a1];
// 写法二，利用剩余操作符
const [...a2] = a1;
```

**四、将字符串打散为数组**
```js
[...'hello']
// [ "h", "e", "l", "l", "o" ]
```

**五、打散实现了 Iterator 接口的对象**

实现了 Iterator 接口的对象比如document.querySelectorAll('div')：
```js
let nodeList = document.querySelectorAll('div'); // nodeList对象不是数组，而是一个类似数组对象
let array = [...nodeList]; // 真正的数组
```

**六、打散Map和Set结构**
```js
let map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

let arr = [...map.keys()]; // [1, 2, 3]
```

**七、打散迭代器对象**
```js
const go = function*(){
  yield 1;
  yield 2;
  yield 3;
};

[...go()] // [1, 2, 3]
```

### 剩余操作符的应用（核心就是打包）
与扩展操作符相反，剩余操作符将多个值收集为一个变量，而扩展操作符是将一个数组扩展成多个值。

```js
const [a, ...b] = [1, 2, 3]

console.log(a) // 1
console.log(b) // [2, 3]
```

**1. Rest 参数接受函数的多余参数，组成一个数组，放在形参的最后，形式如下：**
```js
function func(a, b, ...theArgs){
    // ...
}
```


**1. Rest参数和arguments对象的区别：**
rest参数只包括那些没有给出名称的参数，arguments包含所有参数
arguments 对象不是真正的数组，而rest 参数是数组实例，可以直接应用sort, map, forEach, pop等方法
arguments 对象拥有一些自己额外的功能


**3. 从 arguments 转向数组**
Rest 参数简化了使用 arguments 获取多余参数的方法
```js
// arguments 方法
function func(a, b){
    var args = Array.prototype.slice.call(arguments);
    console.log(args)
}

func(1,2)
```
```js
// Rest 方法
function func(a, b, ...args){
    // ...
}
```
注意，rest 参数之后不能再有其他参数(即，只能是最后一个参数)，否则会报错
```js
function func(a, ...b, c) {
    // ...
}
// Rest parameter must be last formal parameter
```

函数的 length 属性，不包括rest参数
```js
(function(a) {}).length     // 1
(function(...a) {}).length      // 0
(function(a, b, ...c)).length   // 2
```

**4. Rest参数可以被结构(通俗一点，将rest参数的数据解析后一一对应)不要忘记参数用[]括起来，因为它是数组**
```js
function f(...[a, b, c]) {  
  return a + b + c;  
}  
  
f(1)          //NaN 因为只传递一个值，其实需要三个值  
f(1, 2, 3)    // 6  
f(1, 2, 3, 4) // 6 (第四值没有与之对应的变量名)
```

---
layout: post
title:  "ES6的扩展运算符和剩余操作符的对比和应用"
author: Kothing
categories: [ javascript, ES6 ]
image: assets/images/16.jpg
rating: 4.5
---

## 扩展运算符

扩展运算符写法是三个点...，写法虽然跟剩余操作符一致，都是...，但是作用可以认为是相反的。

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

- 剩余操作符举例：
```js
function ff (...a) {
    return a[0] + '-' + a[1] + '-' + a[2];
}

ff(1,3,5); // '1-3-5'
```
- 扩展运算符举例：
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
0000000000000000000000000000000000000000000000000000000000000000000000000000000000

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
let nodeList = document.querySelectorAll('div'); // nodeList对象不是数组，而是一个类似数组的对象
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
**一、解构再打包赋值**


**二、函数打包传参**


0000000000000000000000000000000000000000000000000000000000000000000000000000000000

## Writing code blocks

There are two types of code elements which can be inserted in Markdown, the first is inline, and the other is block. Inline code is formatted by wrapping any word or words in back-ticks, `like this`. Larger snippets of code can be displayed across multiple lines using triple back ticks:



#### HTML

```html
<li class="ml-1 mr-1">
    <a target="_blank" href="#">
    <i class="fab fa-twitter"></i>
    </a>
</li>
```

#### CSS

```css
.highlight .c {
    color: #999988;
    font-style: italic; 
}
.highlight .err {
    color: #a61717;
    background-color: #e3d2d2; 
}
```

#### JS

```js
// alertbar later
$(document).scroll(function () {
    var y = $(this).scrollTop();
    if (y > 280) {
        $('.alertbar').fadeIn();
    } else {
        $('.alertbar').fadeOut();
    }
});
```

#### Python

```python
print("Hello World")
```

#### Ruby

```ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
```

#### C

```c
printf("Hello World");
```




![walking]({{ site.baseurl }}/assets/images/8.jpg)

## Reference lists

The quick brown jumped over the lazy.

Another way to insert links in markdown is using reference lists. You might want to use this style of linking to cite reference material in a Wikipedia-style. All of the links are listed at the end of the document, so you can maintain full separation between content and its source or reference.

## Full HTML

Perhaps the best part of Markdown is that you're never limited to just Markdown. You can write HTML directly in the Markdown editor and it will just work as HTML usually does. No limits! Here's a standard YouTube embed code as an example

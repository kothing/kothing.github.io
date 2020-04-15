---
layout: post
title:  "js数组方法reduce()的使用案例"
author: john
categories: [ Javascript ]
image: assets/images/5.jpg
rating: .5
---
reduce()方法是数组的一个实例方法（共有方法），可以被数组的实例对象调用。reduce() 方法接收一个函数作为累加器（accumulator），数组中的每个值（从左到右）开始缩减，最终为一个值。
```js
arr.reduce([callback, initialValue])
```
**案例1：字母游戏**
```js
const anagrams = str => {
    if (str.length <= 2) {
        return str.length === 2 ? [str, str[1] + str[0]] : str;
    }
    return str.split("").reduce((acc, letter, i) => {
        return acc.concat(anagrams(str.slice(0, i) + str.slice(i + 1)).map(val => letter + val));
    }, []);
}

anagrams("abc");    // 结果会是什么呢？
```
reduce负责筛选出每一次执行的首字母，递归负责对剩下字母的排列组合。

**案例2： 累加器**
```js
const sum = arr => arr.reduce((acc, val) => acc + val, 0);
sum([1, 2, 3]);
```

**案例3： 计数器**
```js
const countOccurrences = (arr, value) => arr.reduce((a, v) => v === value ? a + 1 : a + 0, 0);
countOccurrences([1, 2, 3, 2, 2, 5, 1], 1);
```
循环数组，每遇到一个值与给定值相等，即加1，同时将加上之后的结果作为下次的初始值。

**案例4：函数柯里化**
函数柯里化的目的就是为了储存数据，然后在最后一步执行。
```js
const curry = (fn, arity = fn.length, ...args) => arity <= args.length ? fn(...args) : curry.bind(null, fn, arity, ...args);
curry(Math.pow)(2)(10);
curry(Math.min, 3)(10)(50)(2);
```
通过判断函数的参数取得当前函数的length(当然也可以自己指定)，如果所传的参数比当前参数少，则继续递归下面，同时储存上一次传递的参数。


**案例5：数组扁平化**
```js
//方法
const deepFlatten = arr => arr.reduce((a, v) => a.concat(Array.isArray(v) ? deepFlatten(v) : v), []);
//引用
deepFlatten([1, [2, [3, 4, [5, 6]]]]);
```

**案例6：生成菲波列契数组**
```js
//方法
const fibonacci = n => Array(n).fill(0).reduce((acc, val, i) => acc.concat(i > 1 ? acc[i - 1] + acc[i - 2] : i), []);
//使用
fibonacci(5);
```

**案例7：管道加工器**
```js
```

**案例8：中间件**
```js
```

**案例9：edux-actions对state的加工片段**
```js
```

**案例10：数据加工器**
```js
```

**案例11：对象空值判断**
```js
```

**案例12：分组**
```js
```

**案例13：对象过滤**
```js
```

**案例14：数组中删除指定位置的值**
```js
```

**案例15：promise按照顺序执行**
```js
```

**案例16：排序**
```js
```

**案例17：选择**
```js
```

---
layout: post
title:  "React Hook - useReducer示例详解"
author: john
categories: [ React, tutorial ]
image: assets/images/13.jpg
---

`useReducer` 是React提供的一个高级Hook，它不像useEffect、useState、useRef等必须hook一样，没有它我们也可以正常完成需求的开发，但useReducer可以使我们的代码具有更好的可读性、可维护性、可预测性。

### 什么是reducer
`reducer`的概念是伴随着Redux的出现逐渐在JavaScript中流行起来。但我们并不需要学习Redux去了解Reducer。简单来说 reducer是一个函数(state, action) => newState：接收当前应用的state和触发的动作action，计算并返回最新的state。下面是一段伪代码：
```js
//计算器reducer，根据state（当前状态）和action（触发的动作加、减）参数，计算返回newState
function countReducer(state, action) {
    switch(action.type) {
        case 'add':
            return state + 1;
        case 'sub':
            return state - 1;
        default: 
            return state;
    }
}
```
上面例子：state是一个number类型的数值，reducer根据action的类型（加、减）对应的修改state，并返回最终的state。为了刚接触到reducer概念的小伙伴更容易理解，可以将state改为count，但请始终牢记count仍然是state。
```js
function countReducer(count, action) {
    switch(action.type) {
        case 'add':
            return count + 1;
        case 'sub':
            return count - 1;
        default: 
            return count;
    }
}
```

### reducer 的幂等性
从上面的示例可以看到reducer本质是一个纯函数，没有任何UI和副作用。这意味着相同的输入（state、action），reducer函数无论执行多少遍始终会返回相同的输出（newState）。因此通过reducer函数很容易推测state的变化，并且也更加容易单元测试。
```js
expect(countReducer(1, { type: 'add' })).equal(2); // 成功
expect(countReducer(1, { type: 'add' })).equal(2); // 成功
expect(countReducer(1, { type: 'sub' })).equal(0); // 成功
```


### state 和 newState
`state`是当前应用状态对象，可以理解就是我们熟知的React里面的state。

在上面的例子中state是一个基础数据类型，但很多时候state可能会是一个复杂的JavaScript对象，如上例中count有可能只是 state中的一个属性。针对这种场景我们可以使用ES6的结构赋值：
```js
// 返回一个 newState (newObject)
function countReducer(state, action) {
    switch(action.type) {
        case 'add':
            return { ...state, count: state.count + 1; }
        case 'sub':
            return { ...state, count: state.count - 1; }
        default: 
            return count;
    }
}
```
**注意：**

+ reducer处理的state对象必须是immutable，这意味着永远不要直接修改参数中的state对象，reducer函数应该每次都返回一个新的state object。
+ 既然reducer要求每次都返回一个新的对象，我们可以使用ES6中的解构赋值方式去创建一个新对象，并复写我们需要改变的state属性，如上例。

如果state是多层嵌套，解构赋值实现就非常复杂：
```js
function bookReducer(state, action) {
    switch(action.type) {
        // 添加一本书
        case 'addBook':
            return {
                ...state,
                books: {
                    ...state.books,
                    [bookId]: book,
                }
            };
        case 'sub':
            // ....
        default: 
            return state;
    }
}
```

---
layout: post
title:  "React Hook - State Hook示例详解"
author: Kothing
categories: [ Javascript, React ]
image: "https://images.unsplash.com/photo-1541544537156-7627a7a4aa1c?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=a20c472bc23308e390c8ffae3dd90c60&auto=format&fit=crop&w=750&q=80"
rating: 4.5
---

## Hook 是什么？
Hook 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。
Hook 是一个特殊的函数，它可以让你“钩入” React 的特性。例如，useState 是允许你在 React 函数组件中添加 state 的 Hook。


## 什么时候用 Hook？
如果你在编写函数组件并意识到需要向其添加一些 state，以前的做法是必须将其它转化为 class。现在你可以在现有的函数组件中使用 Hook。

**Hook 示例**
```js
import React, { useState } from 'react';

function Example() {
  // 声明一个叫 "count" 的 state 变量, 和一个用于更新 "count" 值的 "setCount" 的函数
  const [count, setCount] = useState(0);

  return (
    <div>
      // 直接使用 "count", 不能使用this
      <p>You clicked {count} times</p>
      // 通过setCount()函数来更新count的state值
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
声明一个叫 "count" 初始值为0 的 state 变量, 和一个用于更新 "count" 值的 "setCount" 的函数，当用户点击按钮后，我们通过调用 `setCount` 来增加 `count`的state值。

**等价的 class 示例**
```js
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```
state 初始值为 `{ count: 0 }` ，当用户点击按钮后，我们通过调用 `this.setState()` 来增加 `state.count`。

### Hook和函数组件
React的函数组件是这样的：
```js
const Example = (props) => {
  // 你可以在这使用 Hook
  return <div />;
}
```
或是这样：
```js
function Example(props) {
  // 你可以在这使用 Hook
  return <div />;
}
```
你可能把它们叫做“无状态组件”，我们更喜欢叫它”函数组件”。`Hook 在 class 内部是不起作用的`。但你可以使用它们来取代 class 。



## 声明 State 变量
在 class 中，我们通过在构造函数中设置 `this.state` 为 `{ count: 0 }` 来初始化 `count` state 为 `0`：
```js
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }
```

在函数组件中，首先引入 React 中 useState方法。`用方括号定义一个state变量名和用于更新state值的方法`，这种 JavaScript 语法叫数组解构。 useState 方法的返回值为 `当前 state` 以及`更新 state 的函数`。这就是我们写 const [count, setCount] = useState() 的原因，且需要成对的获取它们。**在函数组件中，没有 this**，所以我们不能分配或读取 this.state。
```js
import React, { useState } from 'react';

function Example() {
  // 声明一个叫 "count" 的 state 变量, 和一个用于更新 "count" 值的 "setCount" 的函数
  const [count, setCount] = useState(0);
}
```
useState 是一种新方法，它与 class 里面的 this.state 提供的功能完全相同。
你可能注意到我们用方括号定义了一个 state 变量，等号左边名字并不是 React API 的部分，你可以自己取名字：
```js
const [fruit, setFruit] = useState('banana');
```


**声明多个 state 变量**
```js
const [age, setAge] = useState(42);
const [fruit, setFruit] = useState('banana');
const [todos, setTodos] = useState([{ text: '学习 Hook' }]);
```



## 读取 State
当我们想在 class组件 中显示当前的 count，我们读取 this.state.count：
```js
<p>You clicked {this.state.count} times</p>
```

在函数组件里中，我们可以直接用 count:
```js
<p>You clicked {count} times</p>
```



## 更新 State
在 class 中，我们需要调用 this.setState() 来更新 count 值：
```js
<button onClick={() => this.setState({ count: this.state.count + 1 })}>
    Click me
  </button>
```
在函数组件中，我们已经有了 setCount函数 和 count 变量，所以我们不需要 this:
```js
<button onClick={() => setCount(count + 1)}>
    Click me
  </button>
```

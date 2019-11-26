---
layout: post
title:  "React Hook - ueseEffect Hook 示例详解"
author: Kothing
categories: [ Javascript, React, tutorial ]
image: assets/images/1.jpg
---
Hook 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。 Hook 是一个特殊的函数，它可以让你"钩入" React 的特性。Effect Hook 可以让你在函数组件中执行副作用操作。

```js
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  // Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    // Update the document title using the browser API
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

useEffect具体说明作用呢？如果你熟悉 React class 的生命周期函数，你可以把 useEffect Hook 看做 componentDidMount，componentDidUpdate 和 componentWillUnmount 这三个函数的组合。就是当组件加载完成(componentDidMount)、组件state状态值更新(componentDidUpdate)、组件卸载(componentWillUnmount)时候都会执行的副作用函数。

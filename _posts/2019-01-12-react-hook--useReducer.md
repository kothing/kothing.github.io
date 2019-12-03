---
layout: post
title:  "React Hook - useReducer示例详解"
author: Kothing
categories: [ React, tutorial ]
image: assets/images/13.jpg
---

`useReducer` 它接收一个形如 (state, action) => newState 的 reducer，并返回当前的 state 以及与其配套的 dispatch 方法。useState 的替代方案。
在某些场景下，useReducer 会比 useState 更适用，例如 state 逻辑较复杂且包含多个子值，或者下一个 state 依赖于之前的 state 等。并且，使用 useReducer 还能给那些会触发深更新的组件做性能优化，因为你可以向子组件传递 dispatch 而不是回调函数 。

## reducer概念

### 什么是reducer
`reducer`的概念是伴随着Redux的出现逐渐在JavaScript中流行起来。但我们并不需要学习Redux去了解Reducer。简单来说 reducer是一个函数(state, action) => newState：接收当前应用的state和触发的动作action，计算并返回最新的state。以下是用 reducer 重写 useState 一节的计数器示例：
```js
const initialState = {count: 0};

//根据state（当前状态）和action（触发的动作加、减）参数，计算返回newState
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );
}
```
上面例子：state是一个number类型的数值，reducer根据action的类型（加、减）对应的修改state，并返回最终的state。为了刚接触到reducer概念的小伙伴更容易理解，可以将state改为myState，但请始终牢记count仍然是state。
```js
function reducer(myState, action) {
  switch (action.type) {
    case 'increment':
      return {count: myState.count + 1};
    case 'decrement':
      return {count: myState.count - 1};
    default:
      throw new Error();
  }
}
...
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
对于这种复杂state的场景推荐使用[immer](https://github.com/immerjs/immer "immer")等immutable库解决。

### state为什么需要immutable？
+ reducer的幂等性 
我们上文提到过reducer需要保持幂等性，更加可预测、可测试。如果每次返回同一个state，就无法保证无论执行多少次都是相同的结果

+ React中的state比较方案 
React在比较oldState和newState的时候是使用Object.is函数，如果是同一个对象则不会出发组件的rerender。
可以参考官方文档bailing-out-of-a-dispatch。


### action 理解
action：用来表示触发的行为。

用type来表示具体的行为类型(登录、登出、添加用户、删除用户等)
用payload携带的数据（如增加书籍，可以携带具体的book信息），我们用上面addBook的action为例：
```js
const action = {
    type: 'addBook',
    payload: {
        book: {
            bookId,
            bookName,
            author,
        }
    }
}
function bookReducer(state, action) {
    switch(action.type) {
        // 添加一本书
        case 'addBook':
            const { book } = action.payload;
            return {
                ...state,
                books: {
                    ...state.books,
                    [book.bookId]: book,
                }
            };
        case 'sub':
            // ....
        default: 
            return state;
    }
}
```
**总结**：
reducer是一个利用action提供的信息，将state从A转换到B的一个纯函数，具有一下几个特点：  
语法：(state, action) => newState  
Immutable：每次都返回一个newState， 永远不要直接修改state对象  
Action：一个常规的Action对象通常有type和payload（可选）组成  
type： 本次操作的类型，也是 reducer 条件判断的依据  
payload： 提供操作附带的数据信息  


## reducer示例

这里使用Login登陆为示例

`seState`的实现方式：
```js
function LoginPage() {
    const [name, setName] = useState(''); // 用户名
    const [pwd, setPwd] = useState(''); // 密码
    const [isLoading, setIsLoading] = useState(false); // 是否展示loading，发送请求中
    const [error, setError] = useState(''); // 错误信息
    const [isLoggedIn, setIsLoggedIn] = useState(false); // 是否登录

    const login = (event) => {
        event.preventDefault();
        setError('');
        setIsLoading(true);
        login({ name, pwd })
            .then(() => {
                setIsLoggedIn(true);
                setIsLoading(false);
            })
            .catch((error) => {
                // 登录失败: 显示错误信息、清空输入框用户名、密码、清除loading标识
                setError(error.message);
                setName('');
                setPwd('');
                setIsLoading(false);
            });
    }
    return ( 
        //  返回页面JSX Element
    )
}
```
上面Demo我们定义了5个state来描述页面的状态，在login函数中当登录成功、失败时进行了一系列复杂的state设置。可以想象随着需求越来越复杂更多的state加入到页面，更多的setState分散在各处，很容易设置错误或者遗漏。

---
layout: post
title:  "React Hook - useContext示例详解"
author: Kothing
categories: [ React, tutorial ]
image: assets/images/12.jpg
---
`useContext`从名字上就可以看出，它是以Hook的方式使用React Context（上下文）。先简单介绍Context的概念和使用方式，更多Context的知识可以[参考官方文档](https://zh-hans.reactjs.org/docs/hooks-reference.html#usecontext, "useContext")。

## context 介绍
官方文档
> Context is designed to share data that can be considered “global” for a tree of React components, such as the current authenticated user, theme, or preferred language.
简单来说Context的作用就是对它所包含的组件树提供全局共享数据的一种技术
```js
// 第一步：创建需要共享的context
const ThemeContext = React.createContext('light');

class App extends React.Component {
  render() {
    // 第二步：使用 Provider 提供 ThemeContext 的值，Provider所包含的子树都可以直接访问ThemeContext的值
    return (
      <ThemeContext.Provider value="dark">
        <Toolbar />
      </ThemeContext.Provider>
    );
  }
}
// Toolbar 组件并不需要透传 ThemeContext
function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

function ThemedButton(props) {
  // 第三步：使用共享 Context
  const theme = useContext(ThemeContext);
  render() {
    return <Button theme={theme} />;
  }
}
```
关于`Context`还有一个比较重要的点是：当Context Provider的value发生变化是，他的所有子级消费者都会rerender。


**useContext实现login**  

[useReducer示例详解](https://kothing.github.io/react-hook-useReducer/)文章结尾提到过使用useReducer，可以帮助我们集中式的处理复杂的state管理。但如果我们的页面很复杂，拆分成了多层多个组件，我们如何在子组件触发这些state变化呢，比如在LoginButton触发登录失败操作？
思考如何利用context将dispatch函数作为context的value，共享给页面的子组件。
```js
// 定义初始化值
const initState = {
    name: '',
    pwd: '',
    isLoading: false,
    error: '',
    isLoggedIn: false,
}
// 定义state[业务]处理逻辑 reducer函数
function loginReducer(state, action) {
    switch(action.type) {
        case 'login':
            return {
                ...state,
                isLoading: true,
                error: '',
            }
        case 'success':
            return {
                ...state,
                isLoggedIn: true,
                isLoading: false,
            }
        case 'error':
            return {
                ...state,
                error: action.payload.error,
                name: '',
                pwd: '',
                isLoading: false,
            }
        default: 
            return state;
    }
}
// 定义 context函数
const LoginContext = React.createContext();
function LoginPage() {
    const [state, dispatch] = useReducer(loginReducer, initState);
    const { name, pwd, isLoading, error, isLoggedIn } = state;
    const login = (event) => {
        event.preventDefault();
        dispatch({ type: 'login' });
        login({ name, pwd })
            .then(() => {
                dispatch({ type: 'success' });
            })
            .catch((error) => {
                dispatch({
                    type: 'error'
                    payload: { error: error.message }
                });
            });
    }
    // 利用 context 共享dispatch
    return ( 
        <LoginContext.Provider value={{dispatch}}>
            <...>
            <LoginButton />
        </LoginContext.Provider>
    )
}
function LoginButton() {
    // 子组件中直接通过context拿到dispatch，出发reducer操作state
    const dispatch = useContext(LoginContext);
    const click = () => {
        // 子组件可以直接 dispatch action
        dispatch({
            type: 'error'
            payload: { error: error.message }
        });
    }
}
```
可以看到在useReducer结合useContext，通过context把dispatch函数提供给组件树中的所有组件使用，而不用通过props添加回调函数的方式一层层传递。

**使用Context相比回调函数的优势：**

- 对比回调函数的自定义命名，Context的Api更加明确，我们可以更清晰的知道哪些组件使用了dispatch、应用中的数据流动和变化。这也是React一直以来单向数据流的优势。 

- 更好的性能：如果使用回调函数作为参数传递的话，因为每次render函数都会变化，也会导致子组件rerender。当然我们可以使用useCallback解决这个问题，但相比useCallbackReact官方更推荐使用useReducer，因为React会保证dispatch始终是不变的，不会引起consumer组件的rerender。 

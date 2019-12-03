---
layout: post
title:  "React Hook - useContext示例详解"
author: Kothing
categories: [ Jekyll, Blog, tutorial ]
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


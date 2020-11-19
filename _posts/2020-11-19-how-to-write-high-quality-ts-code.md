---
layout: post
title:  "如何编写高质量TypeScript代码"
author: Kothing
categories: [ Javascript, TypeScript ]
image: assets/images/1.jpg
---

如何编写高质高效的TypeScript代码

### 一、减少重复代码

对于TypeScript新手小伙伴来说，在定义接口时，可能一不小心会出现以下类似的重复代码。比如：
```typescript
interface Person {
  firstName: string;
  lastName: string;
}

interface PersonWithBirthDate {
  firstName: string;
  lastName: string;
  birth: Date;
}
```
很明显，相对于 `Person` 接口来说，`PersonWithBirthDate` 接口只是多了一个 `birth` 属性，其他的属性跟 `Person` 接口是一样的。那么如何避免出现例子中的重复代码呢？要解决这个问题，可以利用 extends 关键字：
```typescript
interface Person { 
  firstName: string; 
  lastName: string;
}

interface PersonWithBirthDate extends Person { 
  birth: Date;
}
```

当然除了使用 extends 关键字之外，也可以使用交叉运算符（&）：
```typescript
type PersonWithBirthDate = Person & { birth: Date };
```

有时候你想要定义一个类型来匹配一个初始对象的属性，比如：
```typescript
// 初始化对象
const INIT_OPTIONS = {
  width: 640,
  height: 480,
  color: "#00FF00",
  label: "VGA",
};
// 你可能会这样定义初始化对象的类型：
interface Options {
  width: number;
  height: number;
  color: string;
  label: string;
}
```
其实，想要快速简洁无误的定义一个对象的属性类型时候，可以使用 typeof 操作符：
```typescript
type Options = typeof INIT_OPTIONS;
```


而在使用可辨识联合（Discriminated Unions）的过程中，也可能出现重复代码。比如：
```typescript
interface SaveAction { 
  type: 'save';
  // ...
}

interface LoadAction {
  type: 'load';
  // ...
}

type Action = SaveAction | LoadAction;
type ActionType = 'save' | 'load'; // Repeated types!
```
为了避免重复定义 `'save'` 和 `'load'`，我们可以使用成员访问语法，来提取对象中属性的类型：
```typescript
type ActionType = Action['type']; // 类型是 "save" | "load"
```
然而在实际的开发过程中，重复的类型并不总是那么容易被发现。有时它们会被语法所掩盖。比如有多个函数拥有相同的类型签名：
```typescript
function get(url: string, opts: Options): Promise<Response> { /* ... */ } 
function post(url: string, opts: Options): Promise<Response> { /* ... */ }
```
对于上面的 get 和 post 方法，为了避免重复的代码，你可以提取统一的类型签名：
```typescript
type HTTPFunction = (url: string, opts: Options) => Promise<Response>; 

const get: HTTPFunction = (url, opts) => { /* ... */ };
const post: HTTPFunction = (url, opts) => { /* ... */ };
```



### 二、使用更精确的类型替代字符串类型


### 三、定义的类型总是表示有效的状态


### 四、选择条件类型而不是重载声明


### 五、一次性创建对象


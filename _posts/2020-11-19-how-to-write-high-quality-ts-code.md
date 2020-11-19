---
layout: post
title:  "如何编写高质量TypeScript代码"
author: Kothing
categories: [ Javascript, TypeScript ]
image: assets/images/5.jpg
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
假设你正在构建一个音乐集，并希望为专辑定义一个类型。这时你可以使用 `interface` 关键字来定义一个 `Album` 类型：
```typescript
interface Album {
  artist: string; // 艺术家
  title: string; // 专辑标题
  releaseDate: string; // 发行日期：YYYY-MM-DD
  recordingType: string; // 录制类型："live" 或 "studio"
}
```
对于 `Album` 类型，你希望 `releaseDate` 属性值的格式为 `YYYY-MM-DD`，而 `recordingType` 属性值的范围为 `live` 或 `studio`。但因为接口中 `releaseDate` 和 `recordingType` 属性的类型都是字符串，所以在使用 `Album` 接口时，可能会出现以下问题：
```typescript
const dangerous: Album = {
  artist: "Michael Jackson",
  title: "Dangerous",
  releaseDate: "November 31, 1991", // 与预期格式不匹配
  recordingType: "Studio", // 与预期格式不匹配
};
```
虽然 `releaseDate` 和 `recordingType` 的值与预期的格式不匹配，但此时 TypeScript 编译器并不能发现该问题。为了解决这个问题，你应该为 `releaseDate` 和 `recordingType` 属性定义更精确的类型，比如这样：
```typescript
interface Album {
  artist: string; // 艺术家
  title: string; // 专辑标题
  releaseDate: Date; // 发行日期：YYYY-MM-DD
  recordingType: "studio" | "live"; // 录制类型："live" 或 "studio"
}
```
重新定义 Album 接口之后，对于前面的赋值语句，TypeScript 编译器就会提示以下异常信息：
```typescript
const dangerous: Album = {
  artist: "Michael Jackson",
  title: "Dangerous",
  releaseDate: "November 31, 1991", // Error: 不能将类型“string”分配给类型“Date”
  recordingType: "Studio", // Error: 不能将类型“"Studio"”分配给类型“"studio" | "live"”
};
```
为了解决上面的问题，你需要为 releaseDate 和 recordingType 属性设置正确的类型，比如这样：
```typescript
const dangerous: Album = {
  artist: "Michael Jackson",
  title: "Dangerous",
  releaseDate: new Date("1991-11-31"),
  recordingType: "studio",
};
```


另一个错误使用字符串类型的场景是设置函数的参数类型。
假设你需要写一个函数，用于从一个对象数组（如下源数组）中抽取某个属性的值并保存到数组中，在 Underscore 库中，这个操作被称为 “pluck”。
```javascript
// 源数组
const albums = [
  {
    artist: "Michael Jackson",
    title: "Dangerous",
    releaseDate: new Date("1991-11-31"),
    recordingType: "studio",
  },
  {
    artist: "Eminem",
    title: "Beautiful",
    releaseDate: new Date("2018-10-06"),
    recordingType: "live",
  },
  {
    artist: "Adele",
    title: "Hello",
    releaseDate: new Date("2019-06-10"),
    recordingType: "studio",
  },
];

// 目标数组
const songs = ['Dangerous', 'Beautiful', 'Hello'];
```

要实现该功能，你可能最先想到以下代码：
```typescript
function pluck(record: any[], key: string): any[] {
  return record.map((r) => r[key]);
}
```

对于以上的 pluck 函数并不是很好，因为它使用了 `any` 类型，特别是作为返回值的类型。那么如何优化 pluck 函数呢？首先，可以通过引入一个泛型参数来改善类型签名：
```typescript
function pluck<T>(record: T[], key: string): any[] {
  // Element implicitly has an 'any' type because expression of type 'string' can't be used to index type 'unknown'.
  // No index signature with a parameter of type 'string' was found on type 'unknown'.
  return record.map((r) => r[key]); // Error
}
```
通过以上的异常信息，可知字符串类型的 `key` 不能被作为 `unknown` 类型的索引类型。要从对象上获取某个属性的值，你需要保证参数 `key` 是对象中的属性。  

说到这里相信有一些小伙伴已经想到了 keyof 操作符，它是 TypeScript 2.1 版本引入的，可用于获取某种类型的所有键，其返回类型是联合类型。接着使用 keyof 操作符来完善下 pluck 函数：
```typescript
function pluck<T>(record: T[], key: keyof T) {
  return record.map((r) => r[key]);
}
```

对于第一次完善后的 pluck 函数，你的 IDE 将会为你自动推断出该函数的返回类型：
```typescript
function pluck<T>(record: T[], key: keyof T): T[keyof T][]
```

对于完善后的 pluck 函数，你可以使用前面定义的 Album 类型来测试一下：
```typescript
const albums: Album[] = [
  {
    artist: "Michael Jackson",
    title: "Dangerous",
    releaseDate: new Date("1991-11-31"),
    recordingType: "studio",
  },
  {
    artist: "Eminem",
    title: "Beautiful",
    releaseDate: new Date("2018-10-06"),
    recordingType: "live",
  },
  {
    artist: "Adele",
    title: "Hello",
    releaseDate: new Date("2019-06-10"),
    recordingType: "studio",
  }
];

// let releaseDateArr: (string | Date)[]
let releaseDateArr = pluck(albums, 'releaseDate');
```

示例中的 releaseDateArr 变量，它的类型被推断为 (string | Date)[]，很明显这并不是你所期望的，它的正确类型应该是 Date[]。那么应该如何解决该问题呢？这时你需要引入第二个泛型参数 K，然后使用 extends 来进行约束：
```typescript
function pluck<T, K extends keyof T>(record: T[], key: K): T[K][] {
  return record.map((r) => r[key]);
}

// let releaseDateArr: Date[]
let releaseDateArr = pluck(albums, 'releaseDate');
```

### 三、定义的类型总是表示有效的状态


### 四、选择条件类型而不是重载声明


### 五、一次性创建对象


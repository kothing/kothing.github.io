---
layout: post
title: "TypeScript什么时候要用命名空间"
author: Kothing
categories: [ TypeScript ]
image: assets/images/11.jpg
featured: false
rating: 5
---

## 什么时候要用命名空间?
如果你发现自己写的功能(函数/类/接口等...)越来越多, 你想对他们进行**分组管理就**可以用命名空间, 下面先用"**类**"举例:
```typescript
namespace Tools {
    const TIMEOUT = 100;
 
    export class Ftp {
        constructor() {
            setTimeout(() => {
                console.log('Ftp');
            }, TIMEOUT)
        }
    }
 
    export class Http {
        constructor() {
            console.log('Http');
        }
    }
 
    export function parseURL(){
        console.log('parseURL');
    }
}
```
仔细看你会发现namespace下还有export, export在这里用来表示哪些功能是可以外部访问的:
```typescript
Tools.TIMEOUT // 报错, Tools上没有这个属性
Tools.parseURL() // 'parseURL'
```

最后我们看下编译成js后的代码:
```javascript
"use strict";
var Tools;
(function (Tools) {
    const TIMEOUT = 100;
    class Ftp {
        constructor() {
            setTimeout(() => {
                console.log('Ftp');
            }, TIMEOUT);
        }
    }
    Tools.Ftp = Ftp;
    class Http {
        constructor() {
            console.log('Http');
        }
    }
    Tools.Http = Http;
    function parseURL() {
        console.log('parseURL');
    }
    Tools.parseURL = parseURL;
})(Tools || (Tools = {}));
```
看js代码能发现, 在js中命名空间其实就是一个全局对象. 如果你开发的程序想要暴露一个全局变量就可以用namespace;

## 如何用命名空间来管理类型?
命名空间不仅可以用在逻辑代码中, 也可以用在**类型**, 用来给类型分组:
```typescript
namespace Food {
    export type A = Window;
    export interface Fruits{
        taste: string;
        hardness: number;
    }
 
    export interface Meat{
        taste: string;
        heat: number;
    }
}
 
let meat: Food.Meat;
let fruits: Food.Fruits;
```

## 如何引入写好的命名空间?
有2种方式：
+ 第一种："/// &lt;reference path="xxx.ts" /&gt;" 
+ 第二种：还有就是import

### 通过 "/// &lt;reference path='xxx.ts'/&gt;" 导入
通过reference进行导入相当于xxx.ts文件内的命名空间和当前文件进行了合并:

xxx.ts
```typescript
// xxx.ts
namespace Food {
    export interface Fruits{
        taste: string;
        hardness: number;
    }
}
```

yyy.ts
```typescript
// yyy.ts
<reference path="xxx.ts" />
 
let meat: Food.Meat;
let fruits: Food.Fruits;
```
现在在yyy.ts中我们就可以直接使用xxx.ts中的Food类型了, 而不需要使用import.

### 通过import导入
如果命名空间是用export导出的, 那么使用的时候就不可以用/// <reference/>了, 要用import导入:

xxx.ts
```typescript
// xxx.ts
// 使用export导出
export interface Fruits{
    taste: string;
    hardness: number;
}
 
export interface Meat{
    taste: string;
    heat: number;
}
```

yyy.ts
```typescript
// yyy.ts
import {Food} from './xxx'; // 使用import导入
let meat: Food.Meat;
let fruits: Food.Fruits;
```

## 如何合并多个命名空间
我们知道接口是可以合并的, 命名空间也是可以的, 下面我们把Vegetables类型合并到Food类型中:

xxx.ts
```typescript
// xxx.ts
namespace Food {
    export interface Fruits{
        taste: string;
        hardness: number;
    }
}
```

yyy.ts
```typescript
// yyy.ts
<reference path="xxx.ts" />
namespace Food {
    export interface Vegetables{
        title: string;
        heat: number;
    }
}
 
type Vh = Food.Vegetables['heat'] // number;
```

### export=
如果你的tsconfig中设置了"module": "umd", 那么`export = Food`等价于`export default Food`, `export=`常见于支持umd的插件的声明文件.


## 命名空间在lodash里的应用
其实我们看下那些老牌插件(jq/lodash)里使用namespace特性的代码, 可以发现主要是在声明文件中(xxx.d.ts), 用来表示暴露出来的全局变量(比如lodash的"_").
```typescript
/// <reference path="./common/common.d.ts" />
/// <reference path="./common/array.d.ts" />
/// <reference path="./common/collection.d.ts" />
/// <reference path="./common/date.d.ts" />
/// <reference path="./common/function.d.ts" />
/// <reference path="./common/lang.d.ts" />
/// <reference path="./common/math.d.ts" />
/// <reference path="./common/number.d.ts" />
/// <reference path="./common/object.d.ts" />
/// <reference path="./common/seq.d.ts" />
/// <reference path="./common/string.d.ts" />
/// <reference path="./common/util.d.ts" />

export = _;
export as namespace _;

declare const _: _.LoDashStatic;
declare namespace _ {
    // tslint:disable-next-line no-empty-interface (This will be augmented)
    interface LoDashStatic {}
}

// Backward compatibility with --target es5
declare global {
    // tslint:disable-next-line:no-empty-interface
    interface Set<T> { }
    // tslint:disable-next-line:no-empty-interface
    interface Map<K, V> { }
    // tslint:disable-next-line:no-empty-interface
    interface WeakSet<T> { }
    // tslint:disable-next-line:no-empty-interface
    interface WeakMap<K extends object, V> { }
}
```

## 关于声明文件
上面为了解释命名空间提及了声明文件(xxx.d.ts), 但由于声明(declare)的内容很多,

## 总结
其实如果你的项目直接是用ts写的可能用不上namespace, 毕竟export就可以产生模块, 模块天然就有隔离分组作用.

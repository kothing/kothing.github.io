---
layout: post
title: "Typescript中interface和type区别详解"
author: Kothing
categories: [ TypeScript ]
tags: [ TypeScript ]
image: assets/images/13.jpg
rating: 5
hidden: true
---

# Typescript中interface和type区别详解
官方描述

> An interface can be named in an extends or implements clause, but a type alias for an object type literal cannot.
> An interface can have multiple merged declarations, but a type alias for an object type literal cannot.


## 相同点
**都可以描述一个对象或者函数**  

**interface**  
```
interface User {
  name: string
  age: number
}

interface SetUser {
  (name: string, age: number): void;
}
```

**type**  
```
type User = {
  name: string
  age: number
};

type SetUser = (name: string, age: number)=> void;
```

**都允许拓展（extends）**  

interface 和 type 都可以拓展，并且两者并不是相互独立的，也就是说 interface 可以 extends type, type 也可以 extends interface 。  
`虽然效果差不多，但是两者语法不同。`

interface 继承使用关键字 `extends`  
```
// interface 继承 interface
interface Name { 
  name: string; 
}
interface User extends Name { 
  age: number; 
}

// interface 继承 type
type Name = { 
  name: string; 
}
interface User extends Name { 
  age: number; 
}
```

type 继承使用符号`&`  
```
// type 继承 type
type Name = { 
  name: string; 
}
type User = Name & { age: number  };


// type 继承 interface
interface Name { 
  name: string; 
}
type User = Name & { 
  age: number; 
}
```


## 不同点

**type 可以而 interface 不行**

- type 可以声明基本类型别名，联合类型，元组等类型
```
// 基本类型别名
type Name = string

// 先定义两个interface接口
interface Dog {
    wong();
}
interface Cat {
    miao();
}
// 联合类型 (联合上面两个接口)
type Pet = Dog | Cat

// 具体定义数组每个位置的类型
type PetList = [Dog, Pet]
```

- type 语句中还可以使用 typeof 获取实例的 类型进行赋值
```
// 当你想获取一个变量的类型时，使用 typeof
let div = document.createElement('div');
type B = typeof div
```

**其他骚操作**  
```
type StringOrNumber = string | number;  
type Text = string | { text: string };  
type NameLookup = Dictionary<string, Person>;  
type Callback<T> = (data: T) => void;  
type Pair<T> = [T, T];  
type Coordinates = Pair<number>;  
type Tree<T> = T | { left: Tree<T>, right: Tree<T> };
```

**interface 可以而 type 不行**  

interface 能够声明合并
```
interface User {
  name: string
  age: number
}

interface User {
  sex: string
}


// 相同接口名会合并类型，最终为：
User {
  name: string
  age: number
  sex: string 
}
```
## 总结
一般来说，如果不清楚什么时候用interface/type，能用 interface 实现，就用 interface , 如果不能就用 type 。其他更多详情参看 官方规范文档

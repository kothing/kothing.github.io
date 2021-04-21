---
layout: post
title: "Typescript实用的内置类型"
author: Kothing
categories: [ TypeScript ]
tags: [ TypeScript ]
image: assets/images/photo-32.jpg
rating: 5
hidden: true
---

# Typescript关键字infer的理解与使用

## infer 的常见使用方式
### 1. 简单的类型提取
```
type Unpacked<T> =  T extends (infer U)[] ? U : T;

type T0 = Unpacked<string[]>; // string
type T1 = Unpacked<string>; // string
```
  
### 2. 嵌套的按顺序类型提取
```
type Unpacked<T> =
    T extends (infer U)[] ? U :
    T extends (...args: any[]) => infer U ? U :
    T extends Promise<infer U> ? U :
    T;

type T0 = Unpacked<string>;  // string
type T1 = Unpacked<string[]>;  // string
type T2 = Unpacked<() => string>;  // string
type T3 = Unpacked<Promise<string>>;  // string
type T4 = Unpacked<Promise<string>[]>;  // Promise<string>
type T5 = Unpacked<Unpacked<Promise<string>[]>>;  // string
```

### 3. 使用多个 infer 变量进行类型提取
```
type Foo<T> = T extends { a: infer U, b: infer U } ? U : never;
type T10 = Foo<{ a: string, b: string }>;  // string
type T11 = Foo<{ a: string, b: number }>;  // string | number
```
  

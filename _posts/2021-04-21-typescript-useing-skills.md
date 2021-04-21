---
layout: post
title: "Typescript使用技巧"
author: Kothing
categories: [ TypeScript ]
tags: [ TypeScript ]
image: assets/images/photo-55.jpg
rating: 5
hidden: true
---

# Typescript使用技巧

## 注释
```
/** I'm a nice boy */
interface Person {
  /** A cool name. */
  name: string,
}
```
通过`/** */`形式的注释可以给 TS 类型做标记，编辑器会有更好的提示

## 巧用Record类型
业务中，我们经常会写枚举和对应的映射:
```
type AnimalType = 'cat' | 'dog' | 'fish';
const AnimalMap = {
  cat: { name: '猫', age: 1},
  dog: { name: '狗', age: 2 },
  fish: { name: '鱼', age: 1 },
};
```
但是没有给每个Animal描述设置ts类型，怎么做呢？

使用`Record`:
```
// 动物名
type AnimalType = 'cat' | 'dog' | 'fish';
// 动物属性描述
interface AnimalDescription {
  name: string;
  age: number;
}
// 使用Record给动物的描述定义类型
const AnimalMap: Record<AnimalType, AnimalDescription> = {
  cat: { name: '猫', age: 1},
  dog: { name: '狗', age: 2 },
  fish: { name: '蛙', age: 1 },
};
// 此时 cat、dog、fish的name类型是string，age是number类型
```

## 巧用tsx+extends
在React中.tsx 文件里，泛型可能会被当做 jsx 标签
```
const toArray = <T>(element: T) => [element]; // Error in .tsx file.
```
加 extends 可破
```
const toArray = <T extends {}>(element: T) => [element]; // No errors.
```

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
type AnimalType = 'cat' | 'dog' | 'frog';
const AnimalMap = {
  cat: { name: '猫', icon: ' '},
  dog: { name: '狗', icon: ' ' },
  forg: { name: '蛙', icon: ' ' },
};
```
注意到上面 `forg` 拼错了吗？

`Record` 可以保证映射完整:
```
type AnimalType = 'cat' | 'dog' | 'frog';
interface AnimalDescription { name: string, icon: string }
const AnimalMap: Record<AnimalType, AnimalDescription> = {
  cat: { name: '猫', icon: ' '},
  dog: { name: '狗', icon: ' ' },
  forg: { name: '蛙', icon: ' ' }, // Hey!
};
```

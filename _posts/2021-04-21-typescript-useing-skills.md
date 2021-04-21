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
TypeScript泛型写法<T>在React中被误认为是html标签（类似<div>）

加 extends 可破
```
const toArray = <T extends {}>(element: T) => [element]; // No errors.
```

## 解决类型错误提示问题

- **对象属性不存在错误**:

这种情况一般在于，该对象值TS知道其有明确类型（不是any，如果是any就不会报错了），但是当前要访问的属性不存在与其已知类型结构。这种情况分两种办法解决：
> 如果能修改该值的类型声明，那么添加上缺损值的属性即可；
> 否则，使用 // @ts-ignore 注释，或者使用类型断言，强制为 any 类型：(this.props as any).notExists

- **类型不明确的错误**:

即一个值的类型可能被注解为联合类型，那么在直接访问时，TS无法确定当前值到底属于哪个精确的类型，所以会报告错误。这种情况有以下解决拌饭：

> 使用类型保护(type guards)  
> 使用类型断言  
> 使用 // @ts-ignore 注释  

应该优先考虑类型保护，因为类型保护本质上就是增加代码逻辑，帮助TS理解确定当前类型，所以代码也会更健壮。而后两种办法，除非明确知道此时该值就是确定的类型，否则即使通过了TS编译器，在代码执行阶段，依然有可能出错！

- **值可能不存在的或为undefined的错误**:

这种情况其实是上面提到的类型不明确错误的一种，一般发生在可选属性或者可选参数时。解决这些情况，最简单的就是使用非空类型断言（前提是确认该值确实是非空）：

非空类型断言的形式是在值后面添加半角感叹号：
```
someVar!.toString();
```


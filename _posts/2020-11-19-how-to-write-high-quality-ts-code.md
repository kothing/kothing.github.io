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


### 二、使用更精确的类型替代字符串类型


### 三、定义的类型总是表示有效的状态


### 四、选择条件类型而不是重载声明


### 五、一次性创建对象


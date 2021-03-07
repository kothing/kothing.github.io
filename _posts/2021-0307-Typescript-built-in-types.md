# Typescript实用的内置类型

typescript 提供了很多实用内置的类型，大家安装typescript 的时候，可以在node-module/typescript/lib/文件下面有对js 所有的声明文件，包含es5,es6...到最新的esnext 版本，本篇主要是总结一下对typescript 实用内置类型的笔记，比如 官方文档给出的这些：

- `Exclude<T, U>`：从T中剔除可以赋值给U的类型。
- `Extract<T, U>`：提取T中可以赋值给U的类型。
- `NonNullable<T>`：从T中剔除null和undefined。
- `ReturnType<T>`：获取函数返回值类型。
- `InstanceType<T>`：获取构造函数类型的实例类型。

## 一、`Required<T>` ：将所有属性类型转为必选属性类型
  
  源码实现：把问号减去
```
type Required<T> = {
  [k in keyof T]-? : T[k]
}
```
用法示例：本来User 的属性类型都是可选的，现在变成必选了

```
type User = {
    name?: string
    password?: string
    id?: number
}

let reqUser: Required<User> = {
    name: "Tom",
    password: "123456",
    id: 1
}
```
 

## 二、`Partial<T>`：将所有类型转为可选类型
源码实现：把问号加上
```
type Partial<T> = {
    [k in keyof T]?: T[k]
}
```
用法示例

```
type User = {
    name: string
    password: string
    id: number
}
let user: Partial<User> = {} ;//属性类型为可选，所以不写也不会报错
```
 

## 三、`Readony`： 将所有属性类型转为只读属性选项类型
源码实现: 在属性key 前面加readonly 关键词
```
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
};
```
用法示例：

```
type User = {
    name: string
}

let readOnlyUser: Readonly<User> = {
  name: "12345"
}
readOnlyUser.name = "mike" ;//报错，无法赋值，只读属性，只能初始化赋值
```
 

## 四、`Pick`：从 T 中筛选出 K (大类型中挑选小类型)
源码实现：
```
type Pick<T, K extends keyof T> = {
    [P in K] :T[P]
};
```
用法示例：

```
type Direction  = {
  "LEFT": string,
  'TOP': string,
  "RIGHT": string,
  "BOTTOM": string
}
type resDirection = Pick<Direction, "LEFT" | "TOP">
//resType 结果是： { "LEFT": string, "TOP": string}
let res: resType = {
   LEFT: "left",
   TOP: "top"
}
```
 

## 五、`Record`： 标记 K 中的属性为 T 类型; key=>value 
源码实现:
```
type Record<K extends keyof any, T> = {
    [P in K]: T;
};
```
用法示例:
```
type TestType= Record<'get' | 'post', {'url': string, 'type': string}>
//结果：
{
   'get': {'url': string, 'type': string},
   'post': {'url': string, 'type': string}
}
```
 

## 六、`Exclude<T, U>`： 用于从类型T中去除不在U类型中的成员，（T中有，U中没有）
源码实现：用条件类型实现
```
type Exclude<T, U> = T extends U ? never: T
```
用法示例：

```
//示例一：
type result = Exclude<"a" | "b" | "c" | "d", "a" | "c" | "f"> ;
// 结果: "b" | "d"
// 在这个例子中，因为用到了Exclude这个条件类型，会尝试寻找在T类型中有但在U类型中没有的成员，最后将获取到的Union类型 "b" | "d"

//示例二：
type TestType =  {name: string} | {name: string, age: number} | "a" | "c" | "d"
type result = Exclude<TestType, {name: string} | "c"> ;
//结果: "a" | "d"
```
![Exclude](https://github.com/kothing/Front-end-Interview/raw/main/Img/Exclude.png 'Exclude')  
 

## 七、`Extract<T, U>`：Exclude 的反操作，（取 T U 两者的交集属性）
源码实现: 条件类型实现
```
type Extract<T, U> = T extends U ? T: never
```
用法示例：
```
type result = Extract<"a" | "b" | "c" | "d", "a" | "c" | "f"> ;
结果："a" | "c"
```
 

## 八、`Omit<T, K>`：从类型 T 中 除去指定属性类型 K
源码实现: 利用`pick` + `Exclude` 结合实现
```
type Omit<T, K extends keyof any> = Pick<T, Exclude<keyof T, K>
```
用法示例: 
```
type resType= Omit<{id: number, name: string, age: number}, "age" | "name">; // "{id: number}"
```

## 九、`NonNullable<T>`：从 T 中剔除 null，underfined 类型
源码实现：
```
type NonNullable<T> = T extends null | undefined?never: T
```
用法示例：
```
type resType = NonNullable<string | number | null | undefined> // string | number
```

## 十、`inter` 关键词
1. typescript2.8 新出的语法
2. 在条件语句中作为待推断的类型变量，推断返回值类型
3. 可以将元组变成联合类型
4. 理解好这个用法， Parameters, ReturnType 等内置类型的实现 都用到这个

用法示例：
```
示例1：
type Foo<T> = T extends { a: infer U; b: infer U } ? U : never;
type T1 = Foo<{ a: string; b: string }>; // T1类型为 string
type T2= Foo<{ a: string; b: number }>; // T1类型为 string | number


示例2：元组变联合类型
type I3 = [string,number]
type Tg<T> = T extends (infer R)[] ? R : never
type T4 = Tg<I3>;// T4类型为： string|number
```
 
## 十一、`Parameters<T>`：获取函数参数类型
源码实现:
```
type Parameters<T extends (...args:any) => any> = T extends (...args: infer P) => any ? P:never
 ```
用法示例:
```
interface Ifn {
   (name: string,age: number): void
}
type getParamsType = TParameters<typeof fn> //返回的是元组：[string, number]
```
 

## 十二、`ReturnType`：获取函数返回值类型
源码实现
```
type ReturnType<T extends (...args: any) => any> = T extends (...args: any)=>infer P ? P: never
```
用法示例：
```
 function fn(name: string): string | number{
     return name
 }
 type resType= ReturnType<typeof fn>;//string | number
 ```

## 十三、`ConstructorParamters`：获取构造函数的参数类型
源码实现：
```
type ConstructorParamters<T extends new (...args:any) => any> = T extends new (...args: infer P) => any ? P: never
```
用法示例：
```
 // 示例
 class Y {
     constructor(name: string, age: number){}
 }
 type resType = ConstructorParamters<typeof Y> //[string, number]
 ```

## 十四、`InstanceType`：获取构造函数类型的实例类型(获取一个类的返回类型)
源码实现：
```
 type InstanceType<T extends new (...args: any) => any> = T extends new (...args: any) => infer R ? R : any;
```
```
class Y{
    ID: number | undefined
    GET(){}
    constructor(name: string, age: number){}
}
type resType= InstanceType<typeof Y>
let obj: resType= {
    ID: 1,
    GET(){}
}
```

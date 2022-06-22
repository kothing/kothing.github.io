---
layout: post
title:  "JavaScript中构造函数和普通函数的区别"
author: Kothing
categories: [ Javascript ]
tags: [ Javascript ]
featured: true
image: assets/images/photo-02.jpg
rating: 5
---

1、构造函数也是一个普通函数，创建方式和普通函数一样，但构造函数习惯上首字母大写

2、构造函数和普通函数的区别在于：调用方式不一样。作用也不一样（构造函数用来新建实例对象）

3、调用方式不一样。  
    a. 普通函数的调用方式：直接调用 person();  
    b. 构造函数的调用方式：需要使用new关键字来调用 new Person();  
    
4、构造函数的函数名与类名相同：Person( ) 这个构造函数，Person 既是函数名，也是这个对象的类名

5、内部用this 来构造属性和方法
```js
function Person(name, job, age) {
     this.name=name;
     this.job=job;
     this.age=age;
     this.sayHi=function()
         {
          alert("Hi")
         }
 }
 ```
 
5、构造函数的执行流程

      A、立刻在堆内存中创建一个新的对象

      B、将新建的对象设置为函数中的this

      C、逐个执行函数中的代码

      D、将新建的对象作为返回值

6、普通函数例子：因为没有返回值，所以为undefined
```js
function person() {

}
var per = person();
console.log(per);
// undefined
```

7、构造函数例子：构造函数会马上创建一个新对象，并将该新对象作为返回值返回
```js
function Person() {

}
var per = new Person();
console.log(per);
// [object, Object]
```

8、用`instanceof` 可以检查一个对象是否是一个类的实例，是则返回true；

所有对象都是`Object`对象的后代，所以任何对象和`Object`做`instanceof`都会返回`true`
```js
function Person(name, age, gender) {
    this.name = name;
    this.age = age;
    this.gender = gender;
}

var per = new Person('张三', 18, '男');
console.log(per);
console.log(per.name);
console.log(per.age);
console.log(per instanceof Person);

// 打印结果
[object, Object]
张三
18
true
```

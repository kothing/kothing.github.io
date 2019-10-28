---
layout: post
title:  "ES6的扩展运算符和剩余操作符的对比和应用"
author: Kothing
categories: [ javascript, ES6 ]
image: assets/images/16.jpg
rating: 4.5
---
There are lots of powerful things you can do with the Markdown editor. If you've gotten pretty comfortable with writing in Markdown, then you may enjoy some more advanced tips about the types of things you can do with Markdown!

As with the last post about the editor, you'll want to be actually editing this post as you read it so that you can see all the Markdown code we're using.


## 扩展运算符

扩展运算符写法是三个点...，写法虽然跟剩余操作符一致，都是...，但是作用可以认为是相反的。

扩展运算符的核心就是2个字：打散。

剩余操作符的核心就是2个字：打包。

### 剩余操作符和扩展运算符在赋值方面的对比：
剩余操作符是：表示剩下的**打包**，通常是只可能放在**变量名**前面；一定有赋值或者传值操作：
```
let [a, ...b] = [1,2,3,4,5];
b; // [2,3,4,5]
```
扩展运算符是：表示把打包好的**打散**，可能放在**变量名**前面，也可能放在**值**的前面；可能有赋值操作，也可能没有赋值操作：
```
let a = [1, ...[2,3,4]]; // 注意，既然是先打散然后赋值给一个变量，就需要用[]包起来形成数组
a; // [1, 2, 3, 4]
```
```
console.log(1, ...[2,3,4]); // 1, 2, 3, 4
```


## Writing code blocks

There are two types of code elements which can be inserted in Markdown, the first is inline, and the other is block. Inline code is formatted by wrapping any word or words in back-ticks, `like this`. Larger snippets of code can be displayed across multiple lines using triple back ticks:



#### HTML

```html
<li class="ml-1 mr-1">
    <a target="_blank" href="#">
    <i class="fab fa-twitter"></i>
    </a>
</li>
```

#### CSS

```css
.highlight .c {
    color: #999988;
    font-style: italic; 
}
.highlight .err {
    color: #a61717;
    background-color: #e3d2d2; 
}
```

#### JS

```js
// alertbar later
$(document).scroll(function () {
    var y = $(this).scrollTop();
    if (y > 280) {
        $('.alertbar').fadeIn();
    } else {
        $('.alertbar').fadeOut();
    }
});
```

#### Python

```python
print("Hello World")
```

#### Ruby

```ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
```

#### C

```c
printf("Hello World");
```




![walking]({{ site.baseurl }}/assets/images/8.jpg)

## Reference lists

The quick brown jumped over the lazy.

Another way to insert links in markdown is using reference lists. You might want to use this style of linking to cite reference material in a Wikipedia-style. All of the links are listed at the end of the document, so you can maintain full separation between content and its source or reference.

## Full HTML

Perhaps the best part of Markdown is that you're never limited to just Markdown. You can write HTML directly in the Markdown editor and it will just work as HTML usually does. No limits! Here's a standard YouTube embed code as an example

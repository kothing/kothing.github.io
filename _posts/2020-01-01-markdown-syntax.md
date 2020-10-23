---
layout: post
title:  "Markdown语法"
author: Kothing
categories: [ Web ]
tags: [Markdown]
image: assets/images/11.jpg
description: "Markdown是一种轻量级标记语言，排版语法简洁，让人们更多地关注内容本身而非排版"
rating: 4.5
---

Markdown是一种轻量级标记语言，排版语法简洁，让人们更多地关注内容本身而非排版。它使用易读易写的纯文本格式编写文档，可与HTML混编，可导出 HTML、PDF 以及本身的 .md 格式的文件。因简洁、高效、易读、易写，Markdown被大量使用，如Github、Wikipedia等网站，如各大博客平台：WordPress、Drupal等。

#### 1. 标题
Markdown 支持两种标题的语法，类 Atx 和类 Setext 形式
+ Atx（注意#后面有个空格）
在想要设置为标题的文字前面加#来表示
一个#是一级标题，二个#是二级标题，以此类推。支持六级标题。
注：标准语法一般在#后跟个空格再写文字，貌似简书不加空格也行。

示例：  
{% highlight ruby %}
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
{% endhighlight %}

+ Setext（-与=数目任意，最好三个及以上，比较直观）
上下文标题
示例：
{% highlight ruby %}
AAA
===
BBB
---
{% endhighlight %}

#### 2. 粗体

{% highlight ruby %}
**这是粗体**
__这是粗体__
{% endhighlight %}

#### 3. 斜体

{% highlight ruby %}
*这是斜体*
_这是斜体_
{% endhighlight %}

#### 4. 段落和换行

+ 第一种写法（上图的这是第一段），直接敲两个回车键即可
{% highlight ruby %}
这是第一段

这是第二段
{% endhighlight %}  

+ 第二种写法（上图的这是第二段），在写完一段后敲两个空格，然后回车写下一段
{% highlight ruby %}
这是第一段(后面加两个空格)  
这是第二段
{% endhighlight %}

+ 第三种写法（上图的这是第三段），在写完一段后用HTML的语法：<br />作为换行，然后写下一段
{% highlight ruby %}
这是第三段<br />这是第四段

这是第三段<br />
这是第四段
{% endhighlight %}

#### 5. 分隔线

可以在一行中用三个及以上的星号、减号、等于号、底线来建立分隔线，行内不能有除空格外的其他东西。
{% highlight ruby %}
***
---
===
___
{% endhighlight %}

#### 6. 引言
{% highlight ruby %}
> 用一个 “>” 可以写一个多行的引用
> 还有一种写法就是每一行都用一个 “>” 号
> 这样写比较美观一点
> > 另外一种就是嵌套引用，就像我一样，用两个“>”
{% endhighlight %}

#### 7. 列表

+ 无序列表
无序列表可以在每行开头用`*`星号、`+`加号、`-`减号来表示(记得`*`、`+`、`-`后面要有空格)，也可以三者混合一起，推荐使用相同的字符，避免混乱。
{% highlight ruby %}
* 一朵百合花
+ 两朵百合花
- 三朵百合花
{% endhighlight %}

+ 有序列表
有序列表用数字接着一个英文句点来表示(后面要有空格比如：`1. `)，数字可无序，但还是推荐使用1.、2.，避免混乱。
{% highlight ruby %}
1. 一朵百合花
2. 两朵百合花
3. 三朵百合花
{% endhighlight %}

+ 列表嵌套
上一级和下一级之间敲三个空格即可
{% highlight ruby %}
1. 一朵百合花
2. 两朵百合花


+ 红色的花
+ 紫色的花
+ 蓝色的花
3. 三朵百合花
{% endhighlight %}

#### 8. 超链接

{% highlight ruby %}
语法：
`[超链接名](超链接地址 "超链接title")`
title可加可不加
示例：
[百度](http://baidu.com)
[Missra](http://missra.com)
{% endhighlight %}

#### 9. 图片 

```
![图片alt](图片地址 '图片title')  
//图片alt就是显示在图片下面的文字，相当于对图片内容的解释。  
//图片title是图片的标题，当鼠标移到图片上时显示的内容。title可加可不加  
```

#### 10. 表格

语法：
{% highlight ruby %}
表头|表头|表头
---|:---:|---:
内容|内容|内容
内容|内容|内容

第二行分割表头和内容。
- 有一个就行，为了对齐，多加了几个
文字默认居左
-两边加：表示文字居中
-右边加：表示文字居右
注：原生的语法两边都要用 | 包起来。此处省略
{% endhighlight %}

示例： 

姓名|技能|排行
---|:---:|---:
刘备|哭|大哥
关羽|打|二哥
张飞|骂|三弟


#### 11. 代码

{% highlight ruby %}
单行代码 
`代码内容`

多行代码 
```
代码内容
```
{% endhighlight %}          

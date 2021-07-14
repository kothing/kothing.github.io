---
layout: post
title:  "JavaScript获取Url参数万能方法"
author: Kothing
categories: [ Javascript ]
tags: [Javascript]
image: assets/images/6.jpg
description: "JavaScript获取Url参数方法，包括#号之后的参数"
rating: 4.5
---

url中的参数`?`之后的参数，如今的url中经常会带有#的场景：`http://test.com/path1/?id=123#/path2/path3`，如果参数是在`#`号之前(也就是`?id=123`在`#/path/path`之前)，此时可以使用`window.location.search`的。

但是`?`号参数是在`#`号之后，使用`window.location.search`是获取不到参数的，比如：`http://test.com/path1/##/?id=123`，此时应该使用`window.location.hash`。

因此兼容各种场景的获取参数方法：

```js
function getUrlParam(name) {
  if (typeof name !== 'string') {
    return null;
  }
  const $search = window.location.search;
  const $hash = window.location.hash;
  const reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");
  let r = null;
  if ($search !== '') {
    r = $search.substr(1).match(reg);
  }
  if ($hash !== '' && r === null) {
    if ($hash.indexOf('?') > 0) {
      r = $hash.split("?")[1].match(reg);
    }
    if ($hash.indexOf('?') < 0 && $hash.indexOf('&') > 0) {
      r = $hash.substr($hash.indexOf('&')).match(reg);;
    }
  }
  return r !== null ? unescape(r[2]) : null;
}
```

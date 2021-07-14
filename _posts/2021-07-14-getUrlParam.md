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

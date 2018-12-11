---
title: JavaScript 意義不明
date: 2018-01-04 16:00:00
tags:
    - Javascript
categories: Javascript
---

覺得有趣

```js
var ary = ['a', 'b', 'c'];
// ["a", "b", "c"]

var ary2 = ary.reduce((result, element) => result.concat(Array(3).fill(element)), []);

//["a", "a", "a", "b", "b", "b", "c", "c", "c"]
```
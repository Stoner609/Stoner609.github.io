---
title: JavaScript 用forEach循環中的數組刪除元素
date: 2017-11-29 09:35:46
tags:
    - Javascript
categories: Javascript
---

用forEach循環中的數組刪除元素
<!-- more -->

Array.prototype.splice
---
```js
var pre = document.getElementById('out');

function log(result) {
  pre.appendChild(document.createTextNode(result + '\n'));
}

var review = ['a', 'b', 'c', 'b', 'a'];

review.forEach(function(item, index, object) {
  if (item === 'a') {
    object.splice(index, 1);
  }
});

log(review);
```
```html
<pre id="out"></pre>
```
> b,c,b


刪除數組指定的值後，後面的值會遞補位置，所以會有以下的情況
---
```js
var pre = document.getElementById('out');

function log(result) {
  pre.appendChild(document.createTextNode(result + '\n'));
}

var review = ['a', 'a', 'b', 'c', 'b', 'a', 'a'];

review.forEach(function(item, index, object) {
  if (item === 'a') {
    object.splice(index, 1);
  }
});

log(review);
```
```html
<pre id="out"></pre>
```
> a,b,c,b,a

ES5 - Array.prototype.reduceRight
---
```js
var pre = document.getElementById('out');

function log(result) {
  pre.appendChild(document.createTextNode(result + '\n'));
}

var review = ['a', 'a', 'b', 'c', 'b', 'a', 'a'];

review.reduceRight(function(acc, item, index, object) {
  if (item === 'a') {
    object.splice(index, 1);
  }
}, []);

log(review);
```
```html
<pre id="out"></pre>
```
> b,c,b

ES5 - Array.prototype.forEach
---
```js
var pre = document.getElementById('out');

function log(result) {
  pre.appendChild(document.createTextNode(result + '\n'));
}

var review = ['a', 'a', 'b', 'c', 'b', 'a', 'a'];

review.slice().reverse().forEach(function(item, index, object) {
  if (item === 'a') {
    review.splice(object.length - 1 - index, 1);
  }
});

log(review);
```
```html
<pre id="out"></pre>
```
> b,c,b

ES6 - yield
```js
var pre = document.getElementById('out');

function log(result) {
  pre.appendChild(document.createTextNode(result + '\n'));
}

function* reverseKeys(arr) {
  var key = arr.length - 1;

  while (key >= 0) {
    yield key;
    key -= 1;
  }
}

var review = ['a', 'a', 'b', 'c', 'b', 'a', 'a'];

for (var index of reverseKeys(review)) {
  if (review[index] === 'a') {
    review.splice(index, 1);
  }
}

log(review);
```
```html
<pre id="out"></pre>
```
> b,c,b

後記
---
Array.prototype.filter 但是這不會改變原來的數組，而是創建一個新的數組。

[參考](https://stackoverflow.com/questions/24812930/how-to-remove-element-from-array-in-foreach-loop/24813338#24813338)
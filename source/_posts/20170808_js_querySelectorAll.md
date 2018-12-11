---
title: JavaScript querySelectorAll 使用 forEach
date: 2017-08-08 08:52:46
tags:
    - Javascript
categories: Javascript
---

我們可以使用forEach在JavaScript中輕鬆的循環數組，但是不幸的是querySelectorAll使用上沒有這麼簡單...
<!-- more -->

```js
document.querySelectorAll('.module').forEach(function() {
  /* Only works in Blink browsers and Firefox 50+。 No Safari or IE/Edge support */
});
```
querySelectAll回傳的不是數組，是一個(non-live)的 NodeList，並不是所有Browsers都支援


目前解決的方式
---

```js
var divs = document.querySelectorAll('div');

[].forEach.call(divs, function(v) {
  v.style.color = "red";
});
```
自己在寫內部系統時(只能用IE環境開啟Orz)，是用以上的方式解決目前的問題，但是這會有hack的問題，但目前也不太想改了，維護別人Code加上IE毛特多和版本不確定等等問題 (網管會設定線上的IE版本糙)，筋疲力竭的狀況下選擇不浪費時間，能動就好 (了不起負責)
[參考](https://toddmotto.com/ditch-the-array-foreach-call-nodelist-hack/)



可以用最傳統的方式解決
---

```js
var divs = document.querySelectorAll('div'), i;

for (i = 0; i < divs.length; ++i) {
  divs[i].style.color = "green";
}
```

建議的是自訂方法
---

```js
// forEach method, could be shipped as part of an Object Literal/Module
var forEach = function (array, callback, scope) {
  for (var i = 0; i < array.length; i++) {
    callback.call(scope, i, array[i]); // passes back stuff we need
  }
};

// Usage:
// optionally change the scope as final parameter too, like ECMA5
var myNodeList = document.querySelectorAll('li');
forEach(myNodeList, function (index, value) {
  console.log(index, value); // passes index + value back!
});
```

Firefox可以用的loop方式
---

```js
/* Be warned, this only works in Firefox */

var divs = document.querySelectorAll('div');

for (var div of divs) {
  div.style.color = "blue";
}
```

這是最不推薦且危險的方式
---

```js
NodeList.prototype.forEach = Array.prototype.forEach;
var divs = document.querySelectorAll('div').forEach(function(el) {
  el.style.color = "orange";
})
```

[參考](https://css-tricks.com/snippets/javascript/loop-queryselectorall-matches/)
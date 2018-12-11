---
title: JavaScript Closures(閉包)
date: 2017-12-01 10:30:00
tags:
    - Javascript
categories: Javascript
---

Mozilla 的介紹
`A closure is a special kind of object that combines two things: a function, and the environment in which that function was created。`
`閉包是個特殊的物件，他包含了一個函式，以及函式被建立時所在的環境。`
<!-- more -->

此篇只是簡單的介紹。詳細自行Google

```js
function greet() {
    name = 'Hammad';
    return function () {
        console.log('Hi ' + name);
    }
}
 
greet(); // 什麼都沒發生，沒有錯誤
 
// 從 greet() 中返回的函數保存到 greetLetter 變數中
greetLetter = greet();
 
 // 調用 greetLetter 相當於調用從 greet() 函數中返回的函數
greetLetter(); // logs 'Hi Hammad'

greet()(); // logs 'Hi Hammad'
```

原文參考
---
[深入理解JavaScript中的作用域和上下文](http://www.css88.com/archives/7255)
[Understand JavaScript Closures With Ease](http://javascriptissexy.com/understand-javascript-closures-with-ease/)
[JavaScript 核心概念之作用域和闭包](http://www.css88.com/archives/7262)
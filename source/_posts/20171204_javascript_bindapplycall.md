---
title: JavaScript bind、call()、apply()
date: 2017-12-04 12:04:00
tags:
    - Javascript
categories: Javascript
---

簡單介紹上述三種方法
<!-- more -->

Apply vs. Call vs. Bind Examples
---

`apply()` 跟 `call()` 本質上沒有差異，差別是接收的參數方式不太一樣，也不用太糾結要用哪種方式，如果參數本來就是陣列，用`apply()`就好

`bind()`在低版本的IE不兼容，它和`call()`很相似，接收參數有兩部分，第一個參數是作為上下文對象，第二個對象是個列表，可以接受多參數。

```js
// call()
var person1 = {firstName: 'Jon', lastName: 'Kuperman'};
var person2 = {firstName: 'Kelly', lastName: 'King'};

function say(greeting) {
    console.log(greeting + ' ' + this.firstName + ' ' + this.lastName);
}
say.call(person1, 'Hello'); // Hello Jon Kuperman
say.call(person2, 'Hello'); // Hello Kelly King

// apply()
var person1 = {firstName: 'Jon', lastName: 'Kuperman'};
var person2 = {firstName: 'Kelly', lastName: 'King'};

function say(greeting) {
    console.log(greeting + ' ' + this.firstName + ' ' + this.lastName);
}
say.apply(person1, ['Hello']); // Hello Jon Kuperman
say.apply(person2, ['Hello']); // Hello Kelly King

// bind()
var person1 = {firstName: 'Jon', lastName: 'Kuperman'};
var person2 = {firstName: 'Kelly', lastName: 'King'};

function say() {
    console.log('Hello ' + this.firstName + ' ' + this.lastName);
}

var sayHelloJon = say.bind(person1);
var sayHelloKelly = say.bind(person2);

sayHelloJon(); // Hello Jon Kuperman
sayHelloKelly(); // Hello Kelly King
```

bind()參數的使用
---
call()是把第二個以及之後的參數作為func方法參數傳進去
```js
function func(a, b, c) {
    console.log(a, b, c);
}
var func1 = func.bind(null,'linxin');
func('A', 'B', 'C');            // A B C
func1('A', 'B', 'C');           // linxin A B
func1('B', 'C');                // linxin B C
func.call(null, 'linxin');      // linxin undefined undefined
```

低版本IE實現bind()方法
---
```js
if (!Function.prototype.bind) {
    Function.prototype.bind = function () {
        // 保留原函數
        var self = this,

        // 保留需要綁定的this上下文
        context = [].shift.call(arguments), 
        
        // 剩餘的參數轉為數組
        args = [].slice.call(arguments);

        // 返回一個新函數
        return function () { 
            self.apply(context,[].concat.call(args, [].slice.call(arguments)));
        }
    }
}
```


原文
---
[Apply vs. Call vs. Bind in JavaScript](https://codeplanet.io/javascript-apply-vs-call-vs-bind/)

[深入浅出妙用 Javascript 中 apply、call、bind](http://web.jobbole.com/83642/)

[JavaScript 中 apply 、call 的详解 #7](https://github.com/lin-xin/blog/issues/7)
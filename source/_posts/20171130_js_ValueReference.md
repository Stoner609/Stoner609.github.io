---
title: JavaScript 原始值和引用值(Value vs. Reference)
date: 2017-11-30 15:50:46
tags:
    - Javascript
categories: Javascript
---

了解如何將對象，數組和函數複製並傳遞給函數。要知道引用時複製了什麼。理解原始值是通過複製值來進行複製和傳遞的。
<!-- more -->

這是基礎觀念，學習js時一定要知道的事情~

By Value
---
有5個資料類型 Boolean, null, undefined, String, Number。
我們可以稱它們為primitive types。

宣告變數 a 的時候，a的記憶體會有一個位置存放它
在指定 b 它的值等於 a 的時候，b也會建立一個記憶體位置存放它。a 和 b雖然值是一樣的，但是他們的記憶體位置是不同的。

```js
var a = 3;
var b = a;

console.log(a, b);
// -> 3,3

var x = 10;
var y = 'abc'

var z = x;
var q = y;

x = 5;
y = 'def';

console.log(x, y, z, q);
// -> 5, 'def', 10, 'abc'
```

By Reference
---
資料類型 Array, Function, Object。
我們可以稱它為 Objects。

當宣告變數 a 為一個Object 的時候，記憶體位置會給他一個位置；在指令另一個變數 b 的值等同於 a 的時候，並不會再另外創建一個記憶體位置給 b，而是一樣指定同一個位置給 b，因此如果 a 的值改變的時候，b也會跟著做變化，因為他們是參考同一個位置。

```js
var a = { name: 'Stoner' };
var b;
a = b;

console.log(a.name); //-> 'Stoner'
console.log(b.name); //-> 'Stoner'

a.name = "Rossi";

console.log(a.name); //-> 'Rossi'
console.log(b.name); //-> 'Rossi'

function changeName(obj) {
    obj.name = 'Apple';
}

changeName(a);
console.log(a.name); //-> 'Apple'
console.log(b.name); //-> 'Apple'

/*------- 其他範例 A -------*/
function changeAgeAndReference(person) {
    person.age = 25;
    person = {
      name: 'John',
      age: 50
    };

    return person;
}

var personObj1 = {
    name: 'Alex',
    age: 30
};

var personObj2 = changeAgeAndReference(personObj1);

console.log(personObj1); // -> { name: 'Alex', age: 25 }
console.log(personObj2); // -> { name: 'John', age: 50 }


/*------- 其他範例 B -------*/
var personObj1 = {
    name: 'Alex',
    age: 30
};

var person = personObj1;
person.age = 25;

person = {
    name: 'John',
    age: 50
};

var personObj2 = person;

console.log(personObj1); // -> { name: 'Alex', age: 25 }
console.log(personObj2); // -> { name: 'John', age: 50 }

/*------- 其他範例 C -------*/
var obj = {
    innerObj: {
        x: 9
    }
};
​
var z = obj.innerObj;
​
z.x = 25;
​
console.log(obj.innerObj.x); 
//-> 25

/*------- 其他範例 D -------*/
var obj = {
    arr: [{ x: 17 }]
};
​
var z = obj.arr;
​
//是使用 object literal 的方式來建立物件，則會變成 by Value，新增了一個記憶體的位置。
z = [{ x: 25 }];
​
console.log(obj.arr[0].x);
//-> 17

/*------- 其他範例 E -------*/
var obj = {};
var arr = [];
obj.arr = arr;
arr.push(9);
obj.arr[0] = 17;
​
console.log(obj.arr === [17]);
//-> false

/*------- 其他範例 F -------*/
function fn(item1, item2) {
    if (item2 === undefined) {
        item2 = [];
    }
    
    item2[0] = item1;
    
    return item2;
}

var w = {};
var x = [w];
var y = fn(w);
var z = fn(w, x);

console.log(x === y);
//-> true
```

> 加強補充範例
是使用 object literal 的方式來建立物件，則會變成 by Value，新增了一個記憶體的位置。
```js
var a = { name: 'Stoner' };
var b = a;
console.log(a.name); //-> 'Stoner'
console.log(b.name); //-> 'Stoner'

a = { name: 'Big Bird' };
console.log(a.name); //-> 'Big Bird'
console.log(b.name); //-> 'Stoner'
```


參考資料
---
[Value vs. Reference](https://www.educative.io/collection/page/5679346740101120/5707702298738688/5685265389584384)
[[筆記] 談談JavaScript中by reference和by value的重要觀念](https://pjchender.blogspot.tw/2016/03/javascriptby-referenceby-value.html)
[[筆記] 談談JavaScript中的物件建立(Object) - Part 2 | 利用大括號{}建立物件](https://pjchender.blogspot.tw/2016/01/javascriptobject-part-2.html)
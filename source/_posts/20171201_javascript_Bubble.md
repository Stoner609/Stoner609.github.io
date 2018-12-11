---
title: JavaScript 捕獲與冒泡事件
date: 2017-12-01 11:38:00
tags:
    - Javascript
categories: Javascript
---

事件是會冒泡的，即使同樣的事件會沿著DOM樹逐級觸發，有時候這不是我們希望發生的行為，可以在事件處理函數中阻止它
<!-- more -->

何謂捕獲冒泡?
---
```html
<div id='div' onclick='alert("div");'>
    <ul onclick='alert("ul");'>
        <li onclick='alert("li");'>test</li>
    </ul>
</div>
```
點擊test後，會由div > ul > li，這流程稱為`捕獲`
再來依序觸發 alert('li') > alert('ul') > alert('div')，這流程是`冒泡`
更詳細的介紹說明在 [參考原文] 

防止冒泡事件
---
stopPropagation()：作用是阻止目標元素的冒泡事件，但是不會阻止默認事件。
IE是使用 e.cancelBubble = true，w3c的方法是 e.stopPropagation()。

```js
function myfn(e){
    window.event? window.event.cancelBubble = true : e.stopPropagation();
}
```
```html
<div id='div' onclick='alert("div");'>
    <ul onclick='alert("ul");'>
        <li onclick='alert("li");myfn(this);'>test</li>
    </ul>
</div>
```
點擊test後，只會觸發alert('li')

防止默認事件
---
w3c的方法是 e.preventDefault()，IE則是使用 e.returnValue = false。
preventDefault 作用是取消一個目標元素的默認行為。
但要取消默認行為的前提是，該元素本身就要有默認行為，如果沒有就算用了也沒有效果，element的默認行為?? `<a> <input type='submit'>...等等`，當事件對象的cancelable為false 表示沒有默認行為，調用了preventDefault() 也沒有作用。
```js
// 假定有 <a href="http://caibaojian.com/" id="testA">caibaojian.com</a>
var a = document.getElementById("testA");
a.onclick = function (e) {
    if (e.preventDefault) {
        e.preventDefault();
    } else {
        window.event.returnValue == false;
    }
}
```

return false
---
javascript的return false只會阻止默認行為，用jQuery的話會阻止默認行為又防止對象冒泡。

原生js
```html
<div id='div' onclick='alert("div");'>
    <ul  onclick='alert("ul");'>
        <li id='ul-a' onclick='alert("li");'>
            <a href="http://caibaojian.com/"id="testB">caibaojian.com</a>
        </li>
    </ul>
</div>
```
```js
var a = document.getElementById("testB");
a.onclick = function(){
    return false;
};
```

jQuery 能阻止默認行為又停止冒泡
```html
<div id='div'  onclick='alert("div");'>
    <ul  onclick='alert("ul");'>
        <li id='ul-a' onclick='alert("li");'>
            <a href="http://caibaojian.com/"id="testC">caibaojian.com</a>
        </li>
    </ul>
</div>
```
```js
$("#testC").on('click',function(){
    return false;
});
```

總結使用的方法
---
`當需要停止冒泡行時，可以使用`
```js
function stopBubble(e) { 
//如果提供了事件對象是一個非 IE Browser
if ( e && e.stopPropagation ) 
    //因此它支持W3C的stopPropagation()方法 
    e.stopPropagation(); 
else 
    //否則，我們需要使用IE的方式來取消事件冒泡 
    window.event.cancelBubble = true; 
}
```
`當需要阻止默認行為時，可以使用`
```js
//阻止Browser的默認
function stopDefault( e ) { 
    //阻止默認Browser動作(W3C) 
    if ( e && e.preventDefault ) 
        e.preventDefault(); 
    //IE中阻止函數默認動作的方式 
    else 
        window.event.returnValue = false; 
    return false; 
}
```

補充
---
firefox的event跟IE的不同。
IE的是全域變數，隨時可用；firefox的要用參數引導才能用，是運時時的臨時變數。
在IE/Opera中是window.event，在Firefox中是event；而事件的對象，在IE中是window.event.srcElement，在Firefox中是event.target，Opera中兩者都可用。
```js
function a(e){
    // firefox下 window.event為null, IE下 event為null
    // 下面這兩段是一樣的
    var e = (e) ? e : ((window.event) ? window.event : null); 
    var e = e || window.event;
}
```

參考原文
---
[DOM 的事件傳遞機制：捕獲與冒泡](http://blog.techbridge.cc/2017/07/15/javascript-event-propagation/)
[JavaScript停止冒泡和阻止浏览器默认行为](http://caibaojian.com/javascript-stoppropagation-preventdefault.html#t1)
---
title: jQuery 的 bind()、on()
date: 2017-12-01 08:50:46
tags:
    - Javascript
    - jQuery
categories: Javascript
---

釐清這兩個的差別
<!-- more -->

Bind()
---
`bind()` 是最直接的事件綁定方法，會綁定事件類型和處理函數到DOM element，此方法解決了大部分Browser的兼容性問題。

```js
$('#Hello li a').bind('click', function(e) { /* todo */ } );
$('#Hello li a').click(function(e) { /* todo */ } );
```
以上兩行執行的事情都是一樣的，它會把所有符合條件的element都註冊事件，如果element一多會有一些效率上面的問題，在註冊的過程中是很昂貴的，因為每個元素都是執行同一件事情，卻一次一次的執行註冊事件。

`優點`
- 解決大多數Browser兼容性的問題。
- 方便簡單直接綁定事件再element上。
- .click() .hover() 這些註冊事件都是bind的一種簡化處理方式。

`缺點`
- element元素一多會有效率上的問題。
- 會註冊所有符合條件的element。
- 沒辦法註冊動態產生後的新element。(這時候就要用delegate去註冊)
- 頁面加載完才會bind()，可能會有效率問題。

On()
---
`bind()`、`live()`、`delegate()`都可以通過on()來實現，`unbind()`、`die()`、`undelegate()`可用`off()`來實現。
`live()` 可以忽略jQuery也不建議用這方法了。
```js
//bind()
$( "#members li a" ).on( "click", function( e ) {} ); 
$( "#members li a" ).bind( "click", function( e ) {} ); 

// Live
$( document ).on( "click", "#members li a", function( e ) {} ); 
$( "#members li a" ).live( "click", function( e ) {} );

// Delegate
$( "#members" ).on( "click", "li a", function( e ) {} ); 
$( "#members" ).delegate( "li a", "click", function( e ) {} );
```

`優點`
- 提供了一種统一註冊事件的方法
- 仍然提供了.delegate()的優點，如果需要你也可以直接用.bind()

[jQuery中的bind(), live(), on(), delegate()](http://www.cnblogs.com/moonreplace/archive/2012/10/09/2717136.html)
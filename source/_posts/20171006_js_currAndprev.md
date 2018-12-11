---
title: JavaScript 輸入框改變前紀錄舊值，並在更新後獲取新值
date: 2017-10-06 17:30:46
tags:
    - Javascript
categories: Javascript
---

輸入框改變前紀錄舊值，並在更新後獲取新值
<!-- more -->


基本的範例
---
```js
$('input').on('focusin', function(){
    console.log("Saving value " + $(this).val());
    $(this).data('val', $(this).val());
});

$('input').on('change', function(){
    var prev = $(this).data('val');
    var current = $(this).val();
    console.log("Prev value " + prev);
    console.log("New value " + current);
});
```

委派的事件處理
---
```js
$(document).on('focusin', 'input', function(){
    console.log("Saving value " + $(this).val());
    $(this).data('val', $(this).val());
}).on('change','input', function(){
    var prev = $(this).data('val');
    var current = $(this).val();
    console.log("Prev value " + prev);
    console.log("New value " + current);
});
```

[參考](https://stackoverflow.com/questions/29118178/input-jquery-get-old-value-before-onchange-and-get-value-after-on-change)
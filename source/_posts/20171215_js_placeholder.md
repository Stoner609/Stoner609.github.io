---
title: JavaScript 使IE也有 Placeholder效果
date: 2017-12-15 10:40:00
tags:
    - Javascript
categories: Javascript
---

其實我也不知道新版IE可不可以支援 HTML5的 Placeholder屬性，
但在公司內部系統開發時，因為IE有設定版本，所以在沒辦法呈現 HTML5的這個預設屬性，只能自己寫了
<!-- more -->


程式碼
---
```js
function add_placeholder(id, placeholder) {
    var el = document.getElementById(id);
    el.placeholder = placeholder;
    el.onfocus = function () {
        if (this.value == this.placeholder) {
            this.value = '';
            el.style.color = '';
        }
    };
    el.onblur = function () {
        if (this.value.length == 0) {
            this.value = this.placeholder;
            el.style.color = '#A9A9A9';
        }
    };
    el.onblur();
}

//如何使用?
add_placeholder('text_1', 'Fuck you ie');
```
[範例網址](https://jsbin.com/larelivahi/edit?html,js,output)

參考
---
已忘記在哪看到的解法

感謝
---
自己
---
title: JavaScript 如何將 querySelectorAll 選取特定元素
date: 2017-11-29 09:30:46
tags:
    - Javascript
categories: Javascript
---

```js
var test = document.querySelectorAll('input[value][type="checkbox"]:not([value=""])');
//get all inputs with the attribute "value" and has the attribute "value" that is not blank.
```
<!-- more -->

以下是範例
---
- .html
```html
<input type="checkbox" id="c2" name="c2" value="DE039230952"/>
<input type="checkbox" id="c2" name="c2" value=""/>
<input type="checkbox" id="c2" name="c2" />
<input type="text" id="c2" name="c2"  value="DE039230952"/>
<input type="radio" id=
```
- .css
```css
input{
    display:block;
    margin:20px;
}
```

- .js
```js
var test = document.querySelectorAll('input[value][type="checkbox"]:not([value=""])');

for (var i = 0; i < test.length; i++) {
    test[i].disabled = "disabled"
}
```

[參考](https://stackoverflow.com/questions/10777684/how-to-use-queryselectorall-only-for-elements-that-have-a-specific-attribute-set)
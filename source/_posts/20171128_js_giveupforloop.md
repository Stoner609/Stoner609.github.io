---
title: JavaScript 棄用For迴圈，擁抱Reduce、ForEach、Filter、Map
date: 2017-11-28 15:30:46
tags:
    - Javascript
categories: Javascript
---

嘗試不再使用For Loop
<!-- more -->

原文都有詳細的說明差異以及用法，截取我覺得很重要的說明部分

- forEach：遍歷每個元素。
- map：遍歷每個元素，回傳的值會替代原本陣列內的值。
- filter：遍歷每個元素，回傳 true 時，目前的值會保留在陣列內，這會回傳一個新陣列，而不是修改原本的陣列。
- reduce：遍歷每個元素，依序組合、加總，然後丟給下個元素，最終會回傳一個結果。

test
[原文](https://yami.io/reduce-foreach-filter-map/)
---
title: ASP.NET - 結構(Struct)、類別(Class)
date: 2017-11-28 14:30:46
tags:
    - ASP.NET
categories: ASP.NET
---

[原文](http://slashlook.com/articles_20170328.html)
<!-- more -->

結構（Struct）
---
1. 是一種實質型別（Value Type）。
2. 存放在記憶體的Stack區。
3. 不需要使用new就可以產生一份新的Struct。
4. 若裡面有N個欄位，那麼一定要把N個欄位填滿，這個Struct才可以開始被使用。
5. 不能擁有空參數的建構子（Constructor），若一定要寫建構子，則在建構子內要把所有欄位都指派值進去。
6. 欄位不可以使用初始化賦值，請走建構子一途。

類別（Class）
---
1. 是一種參考型別（Reference Type）。
2. 存放在記憶體的Heap區。
3. 一定要使用new來產生一份新的Instance。
4. 若裡面有N個欄位，那麼不需要把N個欄位填滿，這個Instance才可以開始被使用。
5. 能購擁有空參數的建構子（Constructor）。
6. 欄位可以使用初始化賦值，例如：public string cName = "尚未命名";。
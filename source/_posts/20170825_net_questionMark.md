---
title: ASP.NET - ?的用途
date: 2017-08-25 13:50:46
tags:
    - ASP.NET
categories: ASP.NET
---

問號的用途
<!-- more -->

三元運算子的就不提了，目前看到的其他兩種做法
```csharp
//? 可用於對int,double,bool等無法直接賦值為null的數據類型進行null的賦值，是System.Nullable<T>的縮寫形式。
int? number;

// ?? 運算子稱為 null 聯合運算子。 如果運算元不是 null，則會傳回左方運算元，否則傳回右方運算元。
int? x = null;
int y = x ?? -1;
Response.Write("<hr>" + y.ToString());
// -1
```
[??](https://docs.microsoft.com/zh-tw/dotnet/csharp/language-reference/operators/null-conditional-operator)

[C# 6.0 還有一個是Null運算子](https://docs.microsoft.com/zh-tw/dotnet/csharp/language-reference/operators/null-conditional-operators)
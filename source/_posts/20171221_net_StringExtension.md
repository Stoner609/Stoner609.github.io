---
title: ASP.NET - 擴充
date: 2017-12-21 09:00:00
tags:
    - ASP.NET
categories: ASP.NET
---

常常碰到`DataRow`撈出資料後，要把型別轉來轉去，像是`DateTime` 跟 `String`
後來寫了個擴充就不用每次一直打 `.ToString("yyyy/MM/dd")`
但目前我也只會寫這種簡單的~ ㄎ
<!-- more -->

```csharp
public static class StringExtension
{
    public static string ToStringDateTime(this DateTime myDatetime)
    {
        return myDatetime.ToString("yyyy/MM/dd");
    }
}

protected void Page_Load(object sender, EventArgs e)
{
    string FuckString = DateTime.Now().ToString("yyyy/MM/dd");
    string HelloString = DateTime.ToStringDateTime(); 
}
```

參考
---
Google
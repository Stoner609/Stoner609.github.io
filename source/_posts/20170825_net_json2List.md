---
title: ASP.NET - 讀取.json
date: 2017-08-25 09:50:46
tags:
    - ASP.NET
categories: ASP.NET
---

因為要及時更改一些資料, 所以在資料夾中建了一支.json檔案
<!-- more -->

.json 存放的位置是該專案底下
```json
//位置 C://Stoner/Application/test.json
[
	{
		"KeyWords": ["新增", "刪除", "修改"]
	}
]
```

```csharp
//位置 C://Stoner/Application/Index.aspx.cs
//類別
public class Item 
{
    public List<string> KeyWords { get; set; }
}

string str = Server.MapPath(".");
using ( StreamReader r = new StreamReader(Server.MapPath(".") + "/test.json",System.Text.Encoding.Default) )
{
    string json = r.ReadToEnd();
    List<Item> items = JsonConvert.DeserializeObject<List<Item>>(json);
}
```
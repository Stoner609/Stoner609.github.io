---
title: ASP.NET - Code behind 設定 <meta ...>IE版本
date: 2017-08-08 13:50:46
tags:
    - ASP.NET
categories: ASP.NET
---

除了可以使用「web.config」及「頁面」本身設定以外，後端的設定方式是...
<!-- more -->

「頁面」的基礎宣告優先權較高於「web.config」

1. 「頁面」設定
```html
<meta http-equiv="X-UA-Compatible" content="IE=edge" />
```

2. 「web.config」設定
```csharp
<httpProtocol> 
     <customHeaders> 
          <clear /> 
          <add name="X-UA-Compatible" value="IE=EmulateIE7" /> 
     </customHeaders> 
</httpProtocol>
```

3. 「Code behind」設定
```csharp
protected void Page_Load(object sender, EventArgs e) {
    HtmlMeta myHtmlMeta = new HtmlMeta();
    myHtmlMeta.HttpEquiv = "X-UA-Compatible";
    myHtmlMeta.Content = "IE=EmulateIE8";
    Header.Controls.AddAt(0, myHtmlMeta);
}
```

[參考](http://www.petelepage.com/blog/2009/04/setting-x-ua-compatible-with-asp-net-pages/)
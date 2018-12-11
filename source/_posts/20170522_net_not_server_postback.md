---
title: ASP.NET - 不使用 ServerControl 傳值到後端
date: 2017-05-22 10:18:00
tags:
    - ASP.NET
categories: ASP.NET
---
ASP.NET - 不使用 ServerControl 傳值到後端
<!--more-->


> ## PostBack如何運作的？
ASP.NET Webform會自動Render出一段ASP.NET產生的JavaScript：
```js
function __doPostBack(eventTarget, eventArgument) {
    if (!theForm.onsubmit || (theForm.onsubmit() != false)) {
        theForm.__EVENTTARGET.value = eventTarget;
        theForm.__EVENTARGUMENT.value = eventArgument;
        theForm.submit();
    }
}
```
* eventTarget： PostBack是由哪一個物件所觸發的。
* eventArgument：觸發PostBack時，想要帶回去的argument值。
------
> ## 練習

>.aspx
```html 
<script>
    var DO_POSTBACK = function () {
        var myText = document.getElementById('text').value;

        var obj =  {
            Para: myText,
        };
        var myJson = JSON.stringify(obj);
        __doPostBack('btn', myJson);
    }
</script>
<body>
    <form id="form1" runat="server">
    <div>
        <input type="text" id="text" />
        <input type="button" value="click" onclick="DO_POSTBACK()" />
    </div>
    </form>
</body>
```
------
>.cs
```csharp 
protected void Page_Load(object sender, EventArgs e) {
    if ( IsPostBack ) {
        //控制項ID
        string myTarget = Request.Params["__EVENTTARGET"];

        //PostBack參數
        string myArgs = Request.Params["__EVENTARGUMENT"];

        if ( myArgs != null ) {
            System.Web.Script.Serialization.JavaScriptSerializer objSerializer = new System.Web.Script.Serialization.JavaScriptSerializer();

            ParaClass obj = objSerializer.Deserialize<ParaClass>(myArgs);
                    Response.Write(obj.Para);
        }

    }
}

#region 建構子
public class ParaClass {
    public string Para { get; set; }

    public ParaClass() {
        Para = string.Empty;
    }
}
#endregion

```
<hr>

>曾遇到在Local端測試沒有問題，但是放到線上之後仍然無法POSTBACK的狀況，是因為沒有__doPostBack 的JS Function，需加入`<asp:LinkButton>` 才會產生

>`<asp:LinkButton>` 其實最後輸出的是一個 `<a>` 的 html tag，因為`<a>`其實無法做Postback的動作，所以會產生一個 __doPostBack 的 JS function

參考資料
[格子樑](http://www.allenkuo.com/genericarticle/view1383.aspx)
[91](http://ithelp.ithome.com.tw/articles/10051046)
---
title: ASP.NET - 在WebForm架構下，利用WebMethod實作AJAX
date: 2017-11-28 12:30:46
tags:
    - ASP.NET
categories: ASP.NET
---

希望能夠前後端分離，不再受到ServerControl 以及 Postback的困擾，並且能夠讓前端框架能夠更簡單明瞭的與webform做結合。這部分董大偉老師已實做過Vue.js + Webform，未來如果有時間再嘗試看看能否與React.js 及 Angular做結合。
<!-- more -->

介紹
---
WebMethod 簡單就是讓 HTML的 button 調用後端的 Page class static Method。

如何不透過runat="server"的按鈕，來調用runat="server"的方法？
有很多方法可以呼叫WebForm的__doPostBack [(點我)](https://stoner609.github.io/2017/05/22/20170522_ASPNET/)

伺服器端的方法撰寫方式(Sample.aspx)
---
```C#
[System.Web.Services.WebMethod(enableSession: true)]
public static String TestMethod(int iX, int iY)
{
	HttpContext.Current.Session["TestSession"] = HttpContext.Current.Session["TestSession"] ?? "YA! I got session. " + System.DateTime.Now;
	return Newtonsoft.Json.JsonConvert.SerializeObject(new {
		cName = "中文字串",
		iMoney = iX + iY,
		bSex = false,
		cSession = HttpContext.Current.Session["TestSession"]
	});
}
```

1. WebMethod的驅動方法很簡單，就是設定[System.Web.Services.WebMethod]屬性即可。
2. enableSession: true表示這個方法需要使用到Page.Session。
3. 在靜態方法裡面調用Session，要用HttpContext.Current.Session來取用。
4. 方法中定義的傳入引數，如果跟前端調用時期傳入的JSON不匹配，那就是直接忽略嘍。
5. 打回前端可以設定成String即可（JSON本來就是字串）。
6. 打回前端，可以調用JSON.NET來幫你序列化物件會比較方便。

----

前端的JavaScript程式碼寫法
---
```js
$(function() {
	$('#ButtonCal').click(function() {
		//取得用戶輸入的參數
		var cJSON = { 'iX': parseInt($('#iX').val()), 'iY': parseInt($('#iY').val()) };
		//呼叫API
		$.ajax({
			type: "POST",
			url: "Sample.aspx/TestMethod",
			async: false,
			data: JSON.stringify(cJSON),
			contentType: "application/json; charset=utf-8",
			dataType: "json", //如果要回傳值，請設成 json
			success: function(response) {
				if (response != null && response.d != null) {
					var data = response.d;
					data = $.parseJSON(data);
					$("#uMsg").html(
						data.cName + "<br>" +
						data.iMoney + "<br>" +
						data.bSex + "<br>" +
						data.cSession
					);
				}
			},
			error: function(xhr) {
				//發生錯誤之處理程序
			}
		});
		//如果前端觸發按鈕有被包在Form內的話，中斷它去進行表單送出
		return false;
	});
});
```
1. 從前端界面拿到的資料，組合成JSON字串後，記得使用JSON.stringify()將其序列化。
2. 透過正常的AJAX Post即可調用WebMethod，調用方式是「你這隻程式碼.aspx/你的後端方法名稱」，這很重要！
3. [WebMethod回傳的JSON資料會包一個大腸頭叫做「d」（無言），大腸頭的定義請搜尋本網站。](https://tatmingstudio.blogspot.tw/2009/05/jquery-aspnet-web-service-datad-or-not.html)
4. 收下來的資料，記得使用$.parseJSON()解回實體物件。


[原文](http://slashlook.com/articles_20170308.html)
[參考](http://atic-tw.blogspot.tw/2013/05/jquery-ajax-aspnet.html)
[參考 - 董大偉](http://studyhost.blogspot.tw/2016/12/aspnet-web-3-aspnet-webformsspa.html)
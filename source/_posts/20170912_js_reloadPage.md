---
title: JavaScript 刷新頁面的幾種方法
date: 2017-09-12 17:30:46
tags:
    - Javascript
categories: Javascript
---

首先頁面跳轉、刷新、重定向要看實施這個動作的物件，一般有三個物件：本頁面的刷新跳轉、父頁面的刷新跳轉、最外層頁面的刷新跳轉。
<!-- more -->

一般window.location.href是操作本頁面的位址，parent.location.href是操作父頁面的位址，top.location.href是操作最外層頁面的位址。

Javascript刷新頁面的幾種方法
---

- history.go(0)     :刷新本頁
- history.go(-1)    :回到上一頁
- history.go(-2)    :回到上一頁的上一頁
- location.reload() :刷新本頁
- location=location
- location.assign(location)
- document.execCommand('Refresh')
- window.navigate(location)
- location.replace(location)
- document.URL=location.href
- window.location與assign相同

---
```js
//頁面自動刷新，content中的20指每隔20秒刷新一次頁面
<meta http-equiv="refresh" content="20"/>
```

```js
//頁面自動跳轉，20秒後跳轉到http://www.wyxg.com頁面
<meta http-equiv="refresh" content="20;url=http://www.wyxg.com"/>
```

```js
//頁面自動刷新js版
<script language="javaScript">
    function myrefresh()
    {
        window.location.reload();
    }
    setTimeout('myrefresh()',1000); //指定1秒刷新一次
</script>
```

```js
//JS刷新框架的腳本語句
//如何刷新包含該框架的頁面用 
<script language='javaScript'>
    parent.location.reload();
</script> 
```
```js
//子窗口刷新父窗口
<script language='javaScript'>
    self.opener.location.reload();
</script>
//(　或　<a href="javascript:opener.location.reload()">刷新</a> )
```

```js
//如何刷新另一個框架的頁面用 
<script language='javaScript'>
    parent.另一FrameID.location.reload();
</script>
```


在實際應用的時候，重新刷新頁面通常使用： location.reload() 或者是 history.go(0) 來做。因為這種做法就像是用戶端點F5刷新頁面，所以頁面的method="post"的時候，會出現「網頁過期」的提示。那是因為Session的安全保護機制。可以想到： .net中當調用 location.reload() 方法的時候， aspx頁面此時在服務端記憶體裡已經存在， 因此必定是isPostback 的。
而這時我們希望頁面重新載入一遍而不是重新提交的時候，我們可以用location.replace() 就可以完成此任務。被replace的頁面每次都在服務端重新生成。你可以這麼寫： window.location.replace(location.href)

replace用法跟 location.href 相同，但用 replace 導到下頁後，就不能再回到上一頁.document.location.href和document.location.replace都可以實現從A頁面切換到B頁面，但他們的區別是：用document.location.href切換後，可以退回到原頁面。而用document.location.replace切換後，不可以通過「後退」退回到原頁面。

Javascript代碼：

```js
history.go(0);
//history的另外兩個方法
history.back(); //後退到前一頁
history.forward();//前進到當前頁之後打開的頁
window.location.reload();
window.location = window.location.href;
window.location.assign(window.location.href);//刷新當前頁
window.location.assign("HTTP://www.google.com");//跳轉到HTTP://www.google.com
window.location.replace(window.location.href);//刷新當前頁
window.location.replace("HTTP://www.google.com");//跳轉到HTTP://www.google.com

//父頁面跳轉
parent.window.history.go(0);
parent.window.location.reload();
parent.window.location = window.location.href;
parent.window.location.assign(window.location.href);//刷新父頁面
parent.window.location.assign("HTTP://www.google.com");//父頁面跳轉到HTTP://www.google.com
parent.window.location.replace(window.location.href);//刷新父頁面
parent.window.location.replace("HTTP://www.google.com");//父頁面跳轉到HTTP://www.google.com
///最外層則用top關鍵字
```

[原文](http://ithelp.ithome.com.tw/articles/10190061)
[原文](http://ithelp.ithome.com.tw/articles/10190062)
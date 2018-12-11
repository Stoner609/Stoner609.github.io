---
title: Node.js - Express
date: 2017-11-29 11:50:46
tags:
    - Node.js
categories: Node.js
---

Express：Web應用程式架構，也是Node.js中極為受歡迎的開發框架
[官網連結](http://expressjs.com/)
<!-- more -->


基本安裝 及 HelloWorld 範例
---
1. 請先安裝Node.js，以及建立資料夾目錄，並進入到該目錄底下
```bat
$ mkdir myapp 
$ cd myapp
```

2. 使用npm init 建立package.json。package.json相關設定 [Specifics of npm’s package.json handling](https://docs.npmjs.com/files/package.json)
```bat
$ npm init
```

3. 把Express安裝在myapp目錄底下，並儲存在package.json中
```bat 
$ npm install express --save
```

4. 在目錄底下建立 index.js
index.js
```js
var express = require('express');
var app = express();

app.get('/', function (req, res) {
  res.send('Hello World!');
});

app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
```

5. 啟動執行，然後再 http://localhost:3000/ 查看
```bat
$ node index.js
```

![](/images/expressHelloWorld.JPG)
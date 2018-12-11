---
title: MongoDB 安裝及基本指令
date: 2017-05-09 09:22:46
tags:
    - MongoDB
categories: MongoDB
---
 MongoDB 初學筆記
<!--more-->

### MongoDB 安裝設定
---
下載 [MongoDB](https://docs.mongodb.com/ "官網")

1. 把 MongoDB 安裝在 C:/mongodb。
2. 在 C:/mongodb 資料夾中，新增兩個資料夾命名為 data、log。
3. 在 C:/mongodb/data/　資料夾中，新增一個資料夾命名為 db。
4. 開啟 cmd 到路徑 C:/mongodb/bin。輸入：
```bat 
mongod --directoryperdb --dbpath C:\mongodb\data\db --logpath C:\mpongodb\log\mongodb.log --logappend --rest --install
net start MongoDB
mongo
```

>成功會顯示：
"MongoDB Shell version: 版本"
"connecting to: test"
表示連線成功。

### MongoDB 常用指令
---
>如果只使用mongo 連線至資料庫時，預設會使用 test 這個資料庫
>常用指令:
>1. [db] 列出當前資料庫名稱
>2. [show dbs] 列出資料庫清單
>3. [use XX] 切換資料庫
>4. [db.auth('accounty', 'password')] 登入身分
>5. [help] 顯示協助訊息
>6. [show collections] 列出資料表
>7. [use XX] + [db.dropDatabase()] 移除database


```bat 
>db
test

>use customers
switched to db customers

>db
customers

>show dbs
local 0.000GB

>db.createCollection('customers');
{"ok": 1}

>show dbs
customers 0.000GB
local 0.000GB

>show collections
customers

>use nodeauth
switch to db nodeauth

>db.createCollection('users')
{'ok': 1}

>show collections
users

>db.users.insert({ name: 'Stoner', email: 'stoner7969@gmail.com', username: 'Stoner', password: '1234' });
WriteResult({'nInserted': 1})

>db.users.find() or db.users.find().pretty()

>db.users.update({username: 'Stoner'}, {$set:{email: 'xxx.gmail.com'}})
WriteResult({ 'nMatched': 1, 'nUpserted': 0, 'nModified': 1 })

>db.users.remove({username: 'Stoner'});
WriteResult({'nRemoved': 1});
```
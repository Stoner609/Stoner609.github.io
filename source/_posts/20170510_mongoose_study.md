---
title: Mongoose模組基本施用
date: 2017-05-10 09:22:46
tags:
    - MongoDB
    - NodeJS
categories: MongoDB
---
Mongoose 基本使用
<!--more-->

## Mongoose 安裝
1. 先準備一個乾淨的nodejs專案 node_modules 只需要有 express + body-parser
2. 安装 mongoose
```js
npm install mongoose --save
```
3. 新增 app.js
```js
var express     = require('express');
var app         = express();
var bodyParser  = require('body-parser');
var mongoose    = require('mongoose');

var port = 8080;

app.get('/', function(req, res) {
  res.send('happy to be here');
});
```
上述為前置作業，方便後面的範例演練

---
## Mongoose 基本操作

 新增 Book.model.js
```js 
//引用模塊
var mongoose = require('mongoose');

//制定資料骨架
var Schema = mongoose.Schema;

var BookSchema = new Schema({
  title: String,
  author: String,
  category: String
});

//創建模型
module.exports = mongoose.model('Book', BookSche)
```
>允許的Schem類型有
>+ String
>+ Number
>+ Date
>+ Buffer
>+ Boolean
>+ Mixed
>+ ObjectId
>+ Array

修改 app.js 
```js
var express = require('express');
var app = express();
var bodyParser = require('body-parser');
var mongoose = require('mongoose');
var Book = require('./Book.model');

var port = 8080;
//db位置
var db = 'mongodb://localhost/book'
//連結db
mongoose.connect(db);

app.use(bodyParser.json())
app.use(bodyParser.urlencoded({
  extended: true
}));

app.get('/', function(req, res) {
  res.send('happy to be here');
});
```
再啟動 app.js 前要先開啟 mongoDB 的連線

## Mongoose 實作 1 -讀取資料
先直接操作 MongoDB 去新增資料，cmd 指令中開啟mongoDB的連線，在C:/mongodb/bin/ 下輸入
```bat
>mongo
```
開啟之後，來新增資料
```bat
>use book
switched to db book

>db.books.insert({
>    title: 'Refactoring the DOM',
>    author: 'Joe Blow',
>    category: 'Technology'
>})
>db.books.insert({
>    title: 'Learn Colloquial Speech',
>    author: 'Susie Q',
>    category: 'Humanities'
>})
>db.books.insert({
>    title: 'Study of the Brain',
>    author: 'Matt G',
>    category: 'Health'
>})
```

接著在 app.js 新增
```js
app.get('/books', function(req, res) {
  console.log('getting all books');
  Book.find({})
    .exec(function(err, books) {
      if(err) {
        res.send('error occured')
      } else {
        console.log(books);
        res.json(books);
      }
    });
});
```
接著啟動
```bat
node app.js
```
http://localhost:8080/books 可以看到新增的三筆資料

## Mongoose 實作 2 -讀取單筆資料
app.js 新增
```js
app.get('/books/:id', function(req, res) {
  console.log('getting all books');
  Book.findOne({
    _id: req.params.id
    })
    .exec(function(err, books) {
      if(err) {
        res.send('error occured')
      } else {
        console.log(books);
        res.json(books);
      }
    });
});
```
http://localhost:8080/books/id

## Mongoose 實作 3 -新增資料
app.js 新增
```js
//
app.post('/book', function(req, res) {
  var newBook = new Book();

  newBook.title = req.body.title;
  newBook.author = req.body.author;
  newBook.category = req.body.category;

  newBook.save(function(err, book) {
    if(err) {
      res.send('error saving book');
    } else {
      console.log(book);
      res.send(book);
    }
  });
});
//另外一種新增方式
app.post('/book2', function(req, res) {
  Book.create(req.body, function(err, book) {
    if(err) {
      res.send('error saving book');
    } else {
      console.log(book);
      res.send(book);
    }
  });
});
```
## Mongoose 實作 3 -修改資料
app.js 新增
```js
app.put('/book/:id', function(req, res) {
  Book.findOneAndUpdate({
    _id: req.params.id
    },
    { $set: { title: req.body.title }
  }, {upsert: true}, function(err, newBook) {
    if (err) {
      res.send('error updating ');
    } else {
      console.log(newBook);
      res.send(newBook);
    }
  });
});
```
## Mongoose 實作 4 -刪除資料
app.js 新增
```js
app.delete('/book/:id', function(req, res) {
  Book.findOneAndRemove({
    _id: req.params.id
  }, function(err, book) {
    if(err) {
      res.send('error removing')
    } else {
      console.log(book);
      res.status(204);
    }
  });
});
```

>以上實作都是用 Postman 來進行操作

## 補充

Module：[body-parser](https://github.com/expressjs/body-parser#bodyparserurlencodedoptions) - 這是一個Node.js中間件處理JSON，Raw，文本和URL編碼的表單數據
>In Postman of the 3 options available for content type select "X-www-form-urlencoded" and it should work.
>Also to get rid of error message replace:
>app.use(bodyParser.urlencoded())
>With:
>app.use(bodyParser.urlencoded({
>  extended: true
>}));
>The 'body-parser' middleware only handles JSON and urlencoded data
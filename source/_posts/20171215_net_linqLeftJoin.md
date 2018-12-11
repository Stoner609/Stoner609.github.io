---
title: ASP.NET - Linq語法應用 GroupJoin
date: 2017-12-15 18:00:00
tags:
    - ASP.NET
categories: ASP.NET
---

昨天在工作上要使用 Linq實踐出資料庫語法的 Left Join，起初我以為我寫過了
參照幾個月前寫的程式碼，結果發現當初是要實踐出 Inner Join，所以又找了一下資料
並且記錄一下，不然我很快就會忘記了 
<!-- more -->

比起一般的linq語法，Lambda我更喜歡，下面會先展示Lambda的寫法

```cs
//先建立兩個類別，之後要組合起來的
class BookId
{
    public int Id { get; set; }
    public string BookType { get; set; }
}

class Book
{
    public int Book_Id { get; set; }
    public string Book_Name { get; set; }
}

//給值
BookId[] myBookId = new BookId[]
{
    new BookId {Id = 1, BookType = "English"},
    new BookId {Id = 2, BookType = "Chinese"}
};

Book[] myBook = new Book[]
{
    new Book { Book_Id = 1, Book_Name = "shit school" },
    new Book { Book_Id = 1, Book_Name = "English is good" },
    new Book { Book_Id = 2, Book_Name = "教你說中文" },
    new Book { Book_Id = 2, Book_Name = "從開始到放棄" },
};
```

Lambda
---

```cs
var query = myBookId.GroupJoin(myBook, 
        a => a.Id, b => b.Book_Id, 
        (a, ps) => new { 
            Key = a.BookType, 
            ps = ps 
        });

    foreach (var item in query)
    {
        Response.Write(String.Format("Book Type {0}:", item.Key));

        foreach (var item2 in item.ps )
        {
            Response.Write(item2.Book_Name);
        }
    }
```

linq
---
```cs
var result = from a in myBookId
                join b in myBook
                on a.Id
                equals b.Book_Id into ps
                select new {
                    Key = a.BookType,
                    ps = ps
                };

//我懶了
```

以上 差不多就是這樣

h0 dl3
---
[groupjoin](https://www.dotnetperls.com/groupjoin)
[LINQ自學筆記-語法應用-聚合資料-Join-3、GroupJoin](https://ithelp.ithome.com.tw/articles/10106321)
[LINQ自學筆記-語法應用-聚合資料-DefaultIfEmpty 運算子、實做 Left Outer Join 效果](https://ithelp.ithome.com.tw/articles/10106744)
[linqsamples](http://linqsamples.com/linq-to-objects/join/GroupJoin-linq)

特別感謝
---
Google

心得
--
shit
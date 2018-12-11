---
title: T-SQL 取得群組的第一個
date: 2017-06-14 12:00:00
tags:
    - MSSQL
categories: MSSQL
---
T-SQL - 取得群組的第一個
<!--more-->

```sql
DECLARE @table TABLE (ID nvarchar(10), Memo nvarchar(10))

INSERT INTO @table (ID, Memo) VALUES ('AA', 'A7')
INSERT INTO @table (ID, Memo) VALUES ('AA', 'A2')
INSERT INTO @table (ID, Memo) VALUES ('AA', 'A6')
INSERT INTO @table (ID, Memo) VALUES ('BB', 'B6')
INSERT INTO @table (ID, Memo) VALUES ('BB', 'B5')
INSERT INTO @table (ID, Memo) VALUES ('CC', 'C9')
INSERT INTO @table (ID, Memo) VALUES ('CC', 'C3')

SELECT ID,
(
  SELECT TOP 1 Memo
  FROM  @table AS T2
  WHERE T2.ID = T1.ID
  GROUP BY Memo
) AS [Memo]
FROM @table AS T1
GROUP BY ID
```

> 結果：
![](/images/db/20170614T-SQL.JPG)
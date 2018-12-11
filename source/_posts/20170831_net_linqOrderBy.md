---
title: ASP.NET - Linq自定義OrderBy條件
date: 2017-08-31 10:50:46
tags:
    - ASP.NET
categories: ASP.NET
---

自定義OrderBy條件
<!-- more -->

Linq101 範例
---

```c#
private class CaseInsensitiveComparer : IComparer<string>
{
    public int Compare(string x, string y)
    {
        return string.Compare(x, y, true);
    }
}

[Category("Ordering Operators")]
[Title("OrderBy - Comparer")]
[Description("This sample uses an OrderBy clause with a custom comparer to " +
            "do a case-insensitive sort of the words in an array.")]
[LinkedClass("CaseInsensitiveComparer")]
public void DataSetLinq31() 
{
    var words3 = testDS.Tables["Words3"].AsEnumerable();
    var sortedWords = words3.OrderBy(a => a.Field<string>("word"), new CaseInsensitiveComparer());

    foreach (var dr in sortedWords)
    {
        Console.WriteLine(dr.Field<string>("word"));
    }
}
```
輸出結果：
```
AbAcUs
aPPLE
BlUeBeRrY
bRaNcH
cHeRry
ClOvEr
```

那如果我要把某個特定的條件擺放在最前面的話
---

```csharp
static void Main(string[] args)
{
    // A 和 B 小寫在前面；C 和 D 大寫在前面
    List<String> demo = new List<String>()
    {
        "a" , "A" , 
        "b" , "B" , 
        "C" , "c" ,
        "D" , "d"
    };
 
    // 區分大小寫
    var result = demo.OrderBy(e => e);
 
    // 不區分大小寫
    var result = demo.OrderBy(e => e , StringComparer.CurrentCultureIgnoreCase);
 
    // 自訂排序
    var result = demo.OrderBy(e => e, new CustomOrder());
 
    foreach (var item in result)
    {
        Console.WriteLine(item.ToString());
    }
}
public class CustomOrder : IComparer<String>
{
    public int Compare(string x, string y)
    {
        if (x.Equals("C")) return -1;
        else if (y.Equals("C")) return 1;
 
        return string.Compare(x, y  , true);
        // 或
        return string.Compare(x, y, StringComparison.OrdinalIgnoreCase);
    }
}
//自訂排序 輸出結果：
//C
//a
//A
//b
//B
//c
//d
//D
```

另一個範例
```csharp
public class JJAlwaysOnTop:IComparer<string>
{
    public int Compare(string s1, string s2)
    {
        if (s1.Equals(s2)) return 0;
        if ("JJ".Equals(s1)) return -1;
        else if ("JJ".Equals(s2)) return 1;
        return s1.CompareTo(s2);
    }
}

void Main()
{
    string[] its = {"KK", "RR", "FF", "TT", "CC", "JJ"};
    JJAlwaysOnTop comparer = new JJAlwaysOnTop();
    var query = its.OrderBy (i => i);
    query.Dump("預設排序結果");
    query = its.OrderBy(i => i, comparer);
    query.Dump("自訂排新結果");
}
//預設排序 輸出結果：
//C
//FF
//JJ
//KK
//RR
//TT

//自訂排序 輸出結果：
//JJ
//CC
//FF
//KK
//RR
//TT
```

LINQ101 - 範例 31
[參考](http://jengting.blogspot.tw/2016/04/LINQ-OrderBy-CustomOrderRule.html)
[參考](http://ithelp.ithome.com.tw/articles/10104089)
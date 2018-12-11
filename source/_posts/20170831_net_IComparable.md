---
title: ASP.NET - IComparable和IComparer以及它們的泛型實現
date: 2017-08-31 17:30:46
tags:
    - ASP.NET
categories: ASP.NET
---

IComparable和IComparer以及它們的泛型實現
<!-- more -->

使用過List，當有數值要排序時可以很簡單的使用Sort()，當自訂Class時的時候，要繼承自IComparable，外加實作 CompareTo Method就可以了。
IComparable 是自己建立了類別後，想要默認條件的排序，則是需要用到IComparable當接口

```csharp
class Student : IComparable<Student>
{
    public string Name { get; set; }
    public int Age { get; set; }

    public int CompareTo(Student other)
    {
        return Age.CompareTo(other.Age);
    }
}
```

如果不想用Age 當作是比較依據，就要使用IComparer來實現自定義的比較器
```csharp
class SortName: IComparer<Student>
{
    public int Compare(Student x, Student y)
    {
        return x.Name.CompareTo(y.Name);
    }
}
```

來測試程式碼的結果
```csharp
List<Student> studentList;
studentList = new List<Student>();
studentList.Add(new Student() { Age = 1, Name = "a1" });
studentList.Add(new Student() { Age = 5, Name = "g1" });
studentList.Add(new Student() { Age = 4, Name = "b1" });
studentList.Add(new Student() { Age = 2, Name = "f1" });

studentList.Sort(new SortName());

string myResponse = string.Empty;
foreach (Student item in studentList)
{ 
    myResponse += item.Name + "----" +item.Age.ToString() + "<br/>";
}

Response.Write(myResponse);
//a1----1
//b1----4
//f1----2
//g1----5
```

[參考](http://pvencs.blogspot.tw/2012/07/icomparableicomparer.html)
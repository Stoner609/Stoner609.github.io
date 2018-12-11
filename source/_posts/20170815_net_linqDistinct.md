---
title: ASP.NET - Linq在使用Distinct去除重複資料時如何所指定依據的成員屬性
date: 2017-08-15 10:50:46
tags:
    - ASP.NET
categories: ASP.NET
---

如何使用distinct? 
<!-- more -->

基本程式碼
---

```csharp
//Person類別
public struct Person
{
    public string ID { get; set; }
    public string Name { get; set; } 
	public override string ToString()
	{
		return Name;
	} 
}

//塞資料
var datas = new List<Person>();
int idx = 0;
for ( idx = 0; idx < 5; ++idx )
{
    datas.Add(new Person() { ID = idx.ToString(), Name = "SLM" });
}
datas.Add(new Person() { ID = idx.ToString(), Name = "FuckSLM" });

//Console
private static void ShowDatas<T>(IEnumerable<T> datas)
{
	foreach (var data in datas)
	{
		Console.WriteLine(data.ToString());
	}
}

//然後直接用內建的Distinct過濾，發現根本沒用
var distinctDatas = datas.Distinct();
ShowDatas(distinctDatas);

//	The example displays the following output:
//	SLM
//	SLM
//	SLM
//	SLM
//	SLM
//	FuckSLM
```

為了解決這個問題，我們必須要做個可依照Person.Name去做比較的Compare類別，該Compare類別必須實做IEqualityCompare.Equals與IEqualityCompare.GetHashCode方法，並在呼叫Distinct過濾時將該Compare物件帶入。

```csharp
//加入
public class PersonCompare : IEqualityComparer<Person>
{
	public bool Equals(Person x, Person y)
	{
		return x.Name.Equals(y.Name);
	}

	public int GetHashCode(Person obj)
	{
		return obj.Name.GetHashCode();
	}
}

distinctDatas = datas.Distinct(new PersonCompare());
ShowDatas(distinctDatas);

//	The example displays the following output:
//	SLM
//	FuckSLM
```

但是這樣做代表我們每次碰到新的類別就必須要實現對應的Compare類別，用起來十分的不便。因此有人就提出用泛型加上反射的方式做一個共用的Compare類別。
```csharp
public class PropertyComparer<T> : IEqualityComparer<T>
{
	private PropertyInfo _PropertyInfo;

	public PropertyComparer(string propertyName)
	{
		_PropertyInfo = typeof(T).GetProperty(propertyName,
	BindingFlags.GetProperty | BindingFlags.Instance | BindingFlags.Public);
		if (_PropertyInfo == null)
		{
			throw new ArgumentException(string.Format("{0} is not a property of type {1}.", propertyName, typeof(T)));
		}
	}

	public bool Equals(T x, T y)
	{
		object xValue = _PropertyInfo.GetValue(x, null);
		object yValue = _PropertyInfo.GetValue(y, null);

		if (xValue == null)
			return yValue == null;

			return xValue.Equals(yValue);
		}

		public int GetHashCode(T obj)
		{
			object propertyValue = _PropertyInfo.GetValue(obj, null);

			if (propertyValue == null)
				return 0;
			else
				return propertyValue.GetHashCode();
		}
	}
}

distinctDatas = datas.Distinct(new PropertyComparer<Person>("Name"));
ShowDatas(distinctDatas);	
```

這樣的作法是減少了許多額外的負擔，但是感覺還是少了一條路，用起來也還是必須要建立Compare物件，而且反射也存在著效能的問題，如果每個元素都透過這個Compare去做判斷，感覺處理上也不是很漂亮。所以有人也意識到了這個問題，用擴充方法提供了一條我們比較熟悉的路，可以直接將Lambda帶入以決定元素要怎樣過濾。

```csharp
public static class EnumerableExtender
{
	public static IEnumerable<TSource> Distinct<TSource, TKey>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector)
	{
		HashSet<TKey> seenKeys = new HashSet<TKey>();
		foreach (TSource element in source)
		{
			var elementValue = keySelector(element);
			if (seenKeys.Add(elementValue))
			{
				yield return element;
			}
		}
	}
}

distinctDatas = datas.Distinct(person => person.Name);
ShowDatas(distinctDatas);
```


[參考1](https://dotblogs.com.tw/larrynung/2012/09/18/74901)
[參考2](https://www.codeproject.com/Articles/94272/A-Generic-IEqualityComparer-for-Linq-Distinct)
[參考3](http://www.craigwardman.com/blog/index.php/2008/11/linq-select-distinct-on-custom-class-property/)
[參考4](https://stackoverflow.com/questions/4565106/linq-distinct-with-a-single-comparison-class-and-interface)
[參考5](https://stackoverflow.com/questions/489258/linqs-distinct-on-a-particular-property)
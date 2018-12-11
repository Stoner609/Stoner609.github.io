---
title: 如何使用C#將DataTable轉為自訂物件List<class>
date: 2017-08-04 10:50:46
tags:
    - ASP.NET
categories: ASP.NET
---

之前看到網路上的文章, 是告知如何讓別人更容易直覺地維護我們的程式碼
<!--more-->

DataTable是屬於弱型別, 所以要讓人更容易直覺維護, 可自定義Class
```csharp
//自定義類別
public class UserInfo
{
    public int ID { get; set; }
    public string Name { get; set; }
    public double Weight { get; set; }
    public DateTime UpdateDate { get; set; }
}

//取得資料
static private DataTable GetTable()
{
    DataTable dt = new DataTable();
    dt.columns.Add("ID");
    dt.columns.Add("Name");
    dt.columns.Add("Weight");
    dt.columns.Add("UpdateDate");

    for (int i = 0; i < 5; i++)
    {
        DataRow dr = dt.NewRow();
        dr["ID"] = i;
        dr["Name"] = "User" + i;
        dr["Weight"] = 25;
        dr["UpdateDate"] = DateTime.Now;
        dt.Rows.Add(dr);
    }
    return dt;
}

//實際轉換的function
public static class DataTableExtensions
{
    public static IList<T> ToList<T>(this DataTable table) where T : new()
    {
        IList<PropertyInfo> properties = typeof(T).GetProperties().ToList();
        IList<T> result = new List<T>();

        //取得DataTable所有的row data
        foreach (var row in table.Rows)
        {
            var item = MappingItem<T>((DataRow)row, properties);
            result.Add(item);
        }

        return result;
    }

    private static T MappingItem<T>(DataRow row, IList<PropertyInfo> properties) where T : new()
    {
        T item = new T();
        foreach (var property in properties)
        {
            if (row.Table.Columns.Contains(property.Name))
            {
                //針對欄位的型態去轉換
                if (property.PropertyType == typeof(DateTime))
                {
                    DateTime dt = new DateTime();
                    if (DateTime.TryParse(row[property.Name].ToString(), out dt))
                    {
                        property.SetValue(item, dt, null);
                    }
                    else
                    {
                        property.SetValue(item, null, null);
                    }
                }
                else if (property.PropertyType == typeof(decimal))
                {
                    decimal val = new decimal();
                    decimal.TryParse(row[property.Name].ToString(), out val);
                    property.SetValue(item, val, null);
                }
                else if(property.PropertyType == typeof(double))
                {
                    double val = new double();
                    double.TryParse(row[property.Name].ToString(), out val);
                    property.SetValue(item, val, null);
                }
                else if (property.PropertyType == typeof(int))
                {
                    int val = new int();
                    int.TryParse(row[property.Name].ToString(), out val);
                    property.SetValue(item, val, null);
                }
                else
                {
                    if (row[property.Name] != DBNull.Value)
                    {
                        property.SetValue(item, row[property.Name], null);
                    }
                }
            }
        }
        return item;
    }
}

//使用方式
List<UserInfo> list = new List<UserInfo>(); 
DataTable dt = GetDataTable();
var result = DataTableExtensions.ToList<UserInfo>(dt).ToList();
list = result.OrderBy(c => c.UpdateDate).ToList();
```

參考資料
[kyleshen](http://kyleap.blogspot.tw/2014/01/cdatatablelist.html)
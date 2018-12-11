---
title: ASP.NET - 用 Ajax 取得 DataTable
date: 2017-12-01 18:00:00
tags:
    - ASP.NET
    - jQuery
categories: ASP.NET
---

網路上看到的覺得還滿特別的。資料是DataTable 不用經過JSON.NET拋到前端
<!-- more -->

或許JSON.NET底層也是用這種方式做的也說不定

```html
<input type="button" id="show2" value="Get" />
<div id="Div1"></div>
<div id="Div2"></div>
```

```js
//WebForm1.aspx
$(document).ready(function () {
    $('#show2').click(function () {
        $.ajax({
            type: "post",
            url: "WebForm1.aspx/GetData",
            data: "{}",
            contentType: "application/json; charset=utf-8",
            dataType: "json",
            success: function (response) {
                var mydts = response.d.mydt;
                $('#Div1').html(BuildDetails(mydts));
                $('#Div2').html(BuildTable(mydts));
            },
            failure: function () {
                alert("error")
            }
        });
    });
});
function BuildDetails(dataTable) {
    var content = [];
	for (var row in dataTable) {
        for (var column in dataTable[row]) {
            content.push("<tr>")
            content.push("<td><b>")
            content.push(column)
            content.push("</td></b>")
            content.push("<td>")
            content.push(dataTable[row][column])
            content.push("</td>")
            content.push("</tr>")
        }
    }
    var top = "<table border='1' class='dvhead'>";
    var bottom = "</table>";
    return top + content.join("") + bottom;
}

function BuildTable(dataTable) {
    var headers = [];
	var rows = [];
	//column
	headers.push("<tr>");
	for (var column in dataTable[0])
		headers.push("<td><b>" + column + "</b></td>");
	headers.push("</tr>");
	//row
	for (var row in dataTable) {
        rows.push("<tr>");
		for (var column in dataTable[row]) {
            rows.push("<td>");
			rows.push(dataTable[row][column]);
			rows.push("</td>");
		}
		rows.push("</tr>");
    }
	var top = "<table border='1' class='gvhead'>";
	var bottom = "</table>";
	return top + headers.join("") + rows.join("") + bottom;
}
```

```csharp
//WebForm1.aspx.cs
private static Dictionary<string, object> ToJson(DataTable table)
{
    Dictionary<string, object> d = new Dictionary<string, object>();
    d.Add(table.TableName, RowsToDictionary(table));
    return d;
}

private static List<Dictionary<string, object>> RowsToDictionary(DataTable table)
{
    List<Dictionary<string, object>> objs = new List<Dictionary<string, object>>();
    foreach ( DataRow dr in table.Rows )
    {
        Dictionary<string, object> drow = new Dictionary<string, object>();
        for ( int i = 0; i < table.Columns.Count; i++ )
        {
            drow.Add(table.Columns[i].ColumnName, dr[i]);
        }
        objs.Add(drow);
    }

    return objs;
}

[WebMethod]
public static Dictionary<string, object> GetData()
{
    DataTable dt = new DataTable();
    dt.TableName = "mydt";
    dt.Columns.Add("NAME", typeof(string));
    dt.Columns.Add("AGE", typeof(string));
    dt.Columns.Add("REMARK", typeof(string));

    for ( int i = 0; i < 1; i++ )
    {
        DataRow dr = dt.NewRow();
        dr["NAME"] = "rico" + i.ToString();
        dr["AGE"] = i.ToString();
        dr["REMARK"] = "test_" + i.ToString();
        dt.Rows.Add(dr);
    }

    return ToJson(dt);
}
```

[[C#][ASP.NET]利用jQuery.ajax取得ASP.NET DataTable](https://dotblogs.com.tw/ricochen/2010/12/17/20202)
---
title: JavaScript GroupBy
date: 2018-01-29 14:00:00
tags:
    - Javascript
categories: Javascript
---

目前要處理一個陣列中的資料，但我想把他 group by 後重新整理成一個陣列
<!-- more -->

`sample code`
```js
//想要依照location作為依據重新整理成一份更清楚的明細資料
var myList = [
    {area: "港澳中國", citycode: "HKG", cityname: "香港", location: "港澳"},
    {area: "港澳中國", citycode: "MFM", cityname: "澳門", location: "港澳"},
    {area: "港澳中國", citycode: "PEK", cityname: "北京", location: "中國"},
    {area: "港澳中國", citycode: "SHA", cityname: "上海", location: "中國"},
    {area: "東北亞", citycode: "TYO", cityname: "東京", location: "日本"},
    {area: "東北亞", citycode: "OKA", cityname: "沖繩", location: "日本"},
    {area: "東北亞", citycode: "SEL", cityname: "首爾", location: "南韓"},
]
```

現在要利用 area 作為groupby依據，重新再整理一個新的格式
加入以下片段
```js
//會回傳 Object 型別
Array.prototype.groupBy = function (prop) {
    return this.reduce(function (groups, item) {
        var val = item[prop];
	    groups[val] = groups[val] || [];
	    groups[val].push(item);
	    return groups;
	}, {});
}

var myGroupByList = myList.groupBy('area'); 

console.dir(myGroupByList); //Object 型別
//Result
/*
> Object
  > 東北亞: Array(3)
    > 0: {area: "東北亞", citycode: "TYO", cityname: "東京", location: "日本"}
    > 1: {area: "東北亞", citycode: "OKA", cityname: "沖繩", location: "日本"}
    > 2: {area: "東北亞", citycode: "SEL", cityname: "首爾", location: "南韓"}
  > 港澳中國: Array(4)
    > 0: {area: "港澳中國", citycode: "HKG", cityname: "香港", location: "港澳"}
    > 1: {area: "港澳中國", citycode: "MFM", cityname: "澳門", location: "港澳"}
    > 2: {area: "港澳中國", citycode: "PEK", cityname: "北京", location: "中國"}
    > 3: {area: "港澳中國", citycode: "SHA", cityname: "上海", location: "中國"}
*/
```

把 Object 的型別資料轉成 Array 的型別
```js
var GroupArea = function (object) {
    var myArray = [];
    for (var key in object) {
        myArray.push({
            'area': key,
            'country': object[key]
        })
    }

    return myArray;
}
var newList = GroupArea(myGroupByList);

console.dir(newList); //Array 型別
//Result
/*
> Array(2)
  > 0:
      area: '港澳中國'
      country: Array(4)
       > 0: {area: "港澳中國", citycode: "HKG", cityname: "香港", location: "港澳"}
       > 1: {area: "港澳中國", citycode: "MFM", cityname: "澳門", location: "港澳"}
       > 2: {area: "港澳中國", citycode: "PEK", cityname: "北京", location: "中國"}
       > 3: {area: "港澳中國", citycode: "SHA", cityname: "上海", location: "中國"}
  > 1:
      area: '東北亞'
      country: Array(3)
       > 0: {area: "東北亞", citycode: "TYO", cityname: "東京", location: "日本"}
       > 1: {area: "東北亞", citycode: "OKA", cityname: "沖繩", location: "日本"}
       > 2: {area: "東北亞", citycode: "SEL", cityname: "首爾", location: "南韓"}
*/
```

大guy4這樣
但仔細看資料中又會發現還可以再利用 location 去做 Group by，但這次我只需要用上面寫好的兩種方法就可以做到一樣的效果了 (有點土法煉鋼)
```js
//修改 GroupArea 這方法
var GroupArea = function (object) {
    var myArray = [];
    for (var key in object) {
        myArray.push({
            'area': key,
            'country': GroupCountry(object[key].groupBy('location'))
        })
    }

    return myArray;
}

//新增 GroupCountry這方法
var GroupCountry = function (object) {
    var myArray = [];
    for (var key in object) {
		myArray.push({
			'country': key,
			'citys': object[key]
		})
	}

	return myArray;
}
```

大概結果
![](/images/javascript/20180129_groupby.JPG)


參考
---
[Group By in Javascript](https://www.consolelog.io/group-by-in-javascript)
[How to get all properties values of a Javascript Object (without knowing the keys)?](https://stackoverflow.com/questions/7306669/how-to-get-all-properties-values-of-a-javascript-object-without-knowing-the-key)
[What is the most efficient method to groupby on a JavaScript array of objects?](https://stackoverflow.com/questions/14446511/what-is-the-most-efficient-method-to-groupby-on-a-javascript-array-of-objects)
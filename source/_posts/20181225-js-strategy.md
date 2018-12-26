---
title: React - Handling Events
date: 2018-12-25 16:56:26
tags: 
    - React
categories: React
---

在我對 React 的認知，宣告事件處理的方法大概分三種
<!-- more -->

```javascript
class myClass extends React.Component {
    constructor() {
        this.myFuncB = this.myFuncB.bind(this);
    }

    myFuncA = () => {
        // ...
    }

    myFuncB() {
        // ...
    }

    myFuncC() {
        // ...
    }

    render () {
        return (
            <input type='button' onClick={() => this.myFuncC()} />
        )
    }
}
```

以上推薦寫法是 myFuncA的方式，另外的兩種在每次render的時候，都會建立一個新的 function，可能會有效能上的問題。

此篇文章單純灌水而已... 謝謝自己沒事找事做的堅持
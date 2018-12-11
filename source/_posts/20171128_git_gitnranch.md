---
title: Git - 如何 Clone 遠程分支
date: 2017-11-28 18:30:46
tags:
    - Git
categories: Git
---

如何 Clone 遠程分支 ?
<!-- more -->

從github clone專案下來後，要在分支工作的部分

clone 專案下來後，發現只有一個master
---
```bat
git branch
* master
```

可以在加上 -a 來查看"所有分支" 或 -r 是看"遠程分支"
```bat
git branch -a
 gh-pages
*master

origin/HEAD -> origin/master
origin/gh-pages
origin/master
```

如果想要在分支工作的話，需要創建一個本地分支
```bat
git checkout -b gh-pages origin/gh-pages
git branch
 master
*gh-pages
```

這樣就可以看到分支上的檔案並且修改它



[參考](http://zoomq.qiniudn.com/ZQScrapBook/ZqFLOSS/data/20120730104729/index.html)
[參考](https://gaohaoyang.github.io/2016/07/07/git-clone-not-master-branch/)
[參考](http://www.jianshu.com/p/79cdb8d514ed)

後記
---
目前在用Hexo Blog的時候，都是在公司發佈更新，但在其他地方要更新Blog時滿不方便的。
主要是因為對git指令也不熟，這個問題也一直擱置著。
但最近想要把 hexo 轉成 hugo的時候，發現一段指令是可以同時把尚未deploy的Blog跟靜態的Blog上傳到github上。雖然知道一直都可以這樣，但始終不知道如何操作，但這部分之後再紀錄。
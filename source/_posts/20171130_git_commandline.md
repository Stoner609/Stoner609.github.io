---
title: Git - 常用指令
date: 2017-11-30 11:30:46
tags:
    - Git
categories: Git
---

目前自己常用的指令，以免自己太長時間沒用git會忘記
<!-- more -->


```bat
::複製 Repository
git clone 

::觀看目前的 Repository
git status

::HEAD 表示目前的版本
git reset --hard HEAD

::查看目前分支
git branch

::查看遠程分支
git branch -r

::查看"所有分支" 
git branch -a

:: 新建並切换到該分支
git checkout -b <分支名稱>

::歷史紀錄，都要用小寫的q退出
git log
git log --pretty=oneline

::把要送出的文件放到暫存區 ( Stage )
git add .

::把暫存區 ( Stage ) 裡所有修改的內容送到目前的分支上
git commit -m "內容"

::將程式上傳到github上
git push

::通常在開始工作或要 push 之前，會先從遠端抓取分支
git pull

::使用 git rebase 避免無謂的 merge。https://ihower.tw/blog/archives/3843
git pull --rebase

::退回到上一個版本
git reset --hard HEAD~1

::退回到上上一個版本
git reset --hard HEAD~2

::退回指定的版本號，寫前幾位就可以了，Git 會自動去找
git reset --hard ad41df36b7

::如果退回版本後，又想回到新版本，可用這指令查詢 commit id
git reflog

::檢查差異
git diff
```

[官方文件(中文)](https://git-scm.com/book/zh/v1/%E8%B5%B7%E6%AD%A5)
[Git-Tutorials GIT基本使用教學](https://github.com/twtrubiks/Git-Tutorials)
[使用 git rebase 避免無謂的 merge](https://ihower.tw/blog/archives/3843)

後記
---
保哥說 "學習Git最終還是要回歸到好用的GUI工具，不然很難推廣"
認同但目前先不去選擇之後要用的 GUI工具。

目前尚未多人使用Git(周圍也只有一兩個朋友在用而已)，還在敲打指令的方式去操作Git
也因為用到的指令也少，敲打指令對目前的我來說比較直覺。
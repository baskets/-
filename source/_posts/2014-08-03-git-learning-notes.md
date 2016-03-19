---
layout: post
title: '#Note, Create local repository and upload to Github.'
date: 2014-08-03 08:40
comments: true
categories: git, programming
---
記錄自己學習 git 的筆記，作為日後回顧用

<!--more-->

#建立新的 repository 的方法
**1. 先於自己的 github 帳號下建立 repository**
在上傳至 github 之前，要先做此認證設定，參考 [Set up git.](https://help.github.com/articles/set-up-git/)
**2. 在需要由 git 控管的資料夾下指令，建立 local repository**
```
git init
```
**3. 將所有檔案新增至 staging area**
```
git add .
```
**4. 將所有檔案提交 local repository，並紀錄提交的訊息**
```
git commit -m "first commit"
```
**5. 建立一個名為 origin 且指向你 github repository 的 remote**
```
git remote add origin https://github.com/username/proj.git
```
**6. (非必要)使用 pull 指令和 github repo merge **
*若github repo.上有local repo.未有的檔案時，必須做這行指令，否則無法push至remote (可能會產生 conflict，需處理 conflicts.)
```
git pull origin master
```
**7. push 至 github 上，master 是我們在 local 端的主分支**
```
git push origin master
```
**References:**
[git Branch基礎](http://blog.gogojimmy.net/2012/01/21/how-to-use-git-2-basic-usage-and-worflow/)
[鳥哥的私房菜](http://linux.vbird.org)
[ihower的github教學](http://ihower.tw/git/index.html)

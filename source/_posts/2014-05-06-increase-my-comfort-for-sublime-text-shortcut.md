---
layout: post
title: '為 Sublime Text 增加自己喜歡的快捷鍵'
date: 2014-05-06 11:46
comments: true
categories: sublime text
---
因為有時候需要使用到 Sublime Text 的自動縮排功能，也就是所謂的 reindent
來整理程式碼，但 sublime text 預設並未將 reindent 的功能指定給某一個 shorcut
因此必須要手動地點選 Edit => Line => Reindent，來達成這個功能
對於經常使用的人，是一件頗麻煩的事情

<!--more-->

但所幸，在 sublime text，使用者可以自己定義喜歡的 shortcut
方式如下:
進入 Preference => Key Bindings - User
若您從未修改過，預設應該是長這樣子的
```
[]
```
更改為用 F10 當作 reindent 的快捷鍵
```
[
  { "keys": ["F10"], "command": "reindent"}
 ]
```
當然若要用多組合 shortcut 的方式也可以，以下就是把 cmd+r 設定為 reindent
```
[
  { "keys": ["command+r"], "command": "reindent"}
 ]
```
大功告成! 試著按按看 cmd + r 會發生什麼事情吧!

Ref.
[Sublime Text 3](http://www.sublimetext.com/docs/3/)

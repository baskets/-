---
layout: post
title: '讓 OS X 上的 Firefox 像 Safari 一樣有上一頁動畫 '
date: 2014-11-15 14:13
comments: true
categories:
---
最近改用 Firefox 作為主要的瀏覽器，但在 macbook 上使用雙指向右滑動的時候，上一頁是直接返回上一頁。而不像 Safari 或 Google Chrome 有個回饋能夠讓我知道我正在做返回上一頁的動作，還真是有點不踏實。於是在網路上找是否有 Plugin 可以讓 Firefox 能夠也能擁有此功能。但是發現原來 Firefox 早在 27 版就已經有這個功能了，就如同 Safari 般，在雙指向右滑動上一頁時會有動畫出現，但再我安裝完後，並沒有預設這個功能，還滿奇怪的，但網路上有人提供了解決之道。

網路上的解決方法是

1. 在網址列輸入 about:config
2. 把 browser.snapshot.limit 的 value 設定為非零的數值
3. 重新開啟 Firefox

你的 Firefox 也能像 Safari 般的擁有上一頁的動畫特效了，這樣用起來也舒服許多了。

> 參考 [這篇](http://www.reddit.com/r/apple/comments/1x2zz2/firefox_27_now_supports_safari_style_animated/)

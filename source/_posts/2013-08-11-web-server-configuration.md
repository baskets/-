---
layout: post
title: 'Apache Web Server Routing 設定'
date: 2013-08-11 15:26
comments: true
categories:
---
此文章為記錄如何設定家裡的 Router，使得外部使用者能夠輸入我們的動態 IP，連入自己所架設的網站伺服器
(這裏使用的是 Apache)
<!--more-->

## 一、路由器的設定 ##
首先以 account/pwd. = user/user 的身分進入 Router(192.168.1.1) 設定 Port Forwarding，選擇 ppp0.3，並將服務選為 Web service (Default port 為80，根據自己設定的 Port)，Port 設定為 Apache 所 Listening 的 Port Number 並將 Server Ip 設定為 192.168.1.XX (此為Command Line 下 ipconfig 指令的ipV4位置，輸入此 IP 應能看到自己所架設的網站)，如此一來，當外部的使用者輸入我們電腦的虛擬 IP 位置，才能夠成功的導向到我們本機端的網站。

## 二、httpd-xampp.conf設定檔的設定##
(我用 [xampp](https://www.apachefriends.org/zh_tw/index.html) 這個套裝軟體來架站)
記得設定 httpd-xampp.conf 把 deny all 改為 allow all，但須注意某些 path 不能為allow all，否則別人可能可以連入一些不該被進入的資料夾。

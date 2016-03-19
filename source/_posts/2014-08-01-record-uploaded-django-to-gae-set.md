---
layout: post
title: '有關於於上傳 Django 至 GAE 之設定與步驟'
date: 2014-08-01 14:05
comments: true
categories: gae, programming, django
---
此篇文章是用來記錄我上傳 Django 至 GAE 的步驟以及搜尋到之資源，做個紀錄，以供日後參考。

**我的環境:**
OS X 10.9.4、Python 2.7.5、Django 1.4。
用 Dango 1.4 的原因是因為目前 GAE 只最高只支援到 Django 1.5 且還是實驗狀態，所以選擇 Django 1.4 作為網站開發的 framework。

<!--more-->

先假設我們已經有一個以 Django 1.4 所架設的網站，在 local 端也已經可以正常的運作，並且已經在GAE網頁上 create 好我們的 application，那我們便可以開始來準備 Deploy 我們的專案至GAE了!  

## 下載GAE SDK for Python
首先我們根據您的系統下載Google App Engine SDK for Python，網址如下
[Download GAE SDK for Python](https://developers.google.com/appengine/downloads?hl=zh-tw)

## 設定app.yaml檔案
現在我們的專案結構如下
oursite
|__ oursite
|__ some apps
|__ static
manage.py

我們必須要在我們專案的根目錄下加入一個 app.yaml 檔，以便讓 GAE 知道我們的需求以及設定。
下面是一個 app.yaml 的範例

**A sample of app.yaml**
要設定你的環境版本，以及專案所需的 libraries，並設定 application 為你當初在 GAE 上創立專案時之ID
也就是 http://**your-app-id**.appspot.com 的粗體部分
```
application: your-app-id
version: 2
runtime: python27
api_version: 1
threadsafe: true

libraries:
- name: django
  version: "1.4"
# - name: MySQLdb
#   version: "latest"
# - name: PIL
#   version: "1.1.7"

builtins:
- django_wsgi: on

env_variables:
  DJANGO_SETTINGS_MODULE: 'oursite.settings'

handlers:
- url: /static
  static_dir: static
```

我們必需將 app.yaml 放在我們專案的根目錄之下，如下

oursite
|__ oursite
|__ some apps
|__ static
manage.py
app.yaml

最後都設定完成後，我們就可以下以下的指令 Deploy 我們的網站專案至 GAE。
```
appcfg.py --oauth2 update .
```
> Notes:
若下 appcfg.py 是 command not found，請開啟我們在一開始安裝的 GoogleAppEngineLauncher，點在 Preference 中的 symlink，如此應該能夠解決指令找不到的問題。
更多 appcfg.py 的用法:
[More about appcfg.py](http://www.keakon.net/2009/02/27/appcfg.py%E4%BD%BF%E7%94%A8%E4%BB%8B%E7%BB%8D), [Google Documentation](https://developers.google.com/appengine/docs/python/tools/uploadinganapp?csw=1)


在下完這行 command 之後，會自動開啟您的預設瀏覽器，做帳號的登入與權限確認便會開始進行 deploy 如下圖

<img src="http://user-image.logdown.io/user/3139/blog/3166/post/212300/1h6DGg5CTHSAJcwMVRkw_Screen%20Shot%202014-08-02%20at%200.27.25.png" alt="deploy">

完成之後在 [http://your-app-id.appspot.com](http://your-app-id.appspot.com)，我想就能看見你的網站了，那就大功告成了
上傳之後可能會遇到一些問題，譬如您的 Django 專案可能是有 import 第三方的 module，但 GAE 上並不會有，所以別忘了要一起把需要用到的 modules 或 packages 一同上傳上去，可以參考此[網址](http://stackoverflow.com/questions/14850853/how-to-include-third-party-python-libraries-in-google-app-engine)做設定。

以上，這篇文章大概是一個簡略的流程紀錄。
另外可以在 github 上，搜尋 gae django 上搜尋關鍵字，應該可以找到類似的 examples，讓你更方便地上傳 Django 專案至 GAE 上。

**References:**
[Django-deployer](http://appsembler.com/blog/deploy-django-apps-to-google-app-engine-with-django-deployer-in-5-minutes/) : 一個外國人寫的自動化上傳工具，不過我無法成功，有興趣的人可以參考看看!
[Create a Blog on GAE](http://dev.pippi.im/writing/create-a-blog-in-minutes-on-app-engine-with-django/)

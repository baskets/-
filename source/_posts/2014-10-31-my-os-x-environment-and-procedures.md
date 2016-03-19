---
layout: post
title: '#Note, 我的 OS X 程式環境重建'
date: 2014-10-31 14:02
comments: true
categories:
---
我用的是2012年的 Macbook Air with OS X Yosemite，最近用 Appcleaner 想說清理一些根本不會用到的程式，結果一清，不知原因(可能這並不是一個很好的刪除程式方式...)地導致某些程式無法正常開啟... 嘗試 Google 解法，但不得其解，好吧，只好索性選擇 clean install 系統好了，反正使用了一段時間也多了一些垃圾... 但麻煩的點就在於，開發程式的環境要重新安裝! 好吧，就當作重新複習一次架設環境吧!
<!--more-->
## 一、程式開發環境重建
首先，再重灌之前，我已經先備份了 ~/.bash_profile，因為裡面儲存了一些重要的設定
所以重灌後，直接在 ~/ 底下建立 .bash_profile 就好了。

之後我整理出自己的程式開發需求：
###Django 為網站開發的 Framework，Google Apache Engine 為 Hosting Service.

> 這裏是假設已經有一個可跑的 Django 專案，但原則上做完以下事情，也可以以 Django 架站了。

我現在用 Pycharm 作為主要的 IDE，~~Sublime Text~~ Atom 為輔，而 Terminal 使用 iTerm2 並安裝 zsh 來取代內建的終端機。  
~~Python & easy_install(用以安裝 pip) (已內建)~~  
首先安裝 pip - Python 的 Packages 管理程式。
使用 Homebrew 安裝 Python 就會自帶 pip
```
# 安裝 Homebrew
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
# 安裝 python
$ brew install python
$ which python
/usr/local/bin/python
```
/usr/local/bin/python alias  
/usr/local/Cellar/python/2.7.10/Frameworks/Python.framework/Versions/2.7/bin/python  
這就是 Homebrew 所管理的 Python bin 檔(Homebrew 有自己的安裝路徑(/usr/local/Cellar)，方便管理 brew install 的程式)。而原生的 Python 是 /usr/bin/python (2.7.6版)  
###virtualenv & virtualenvwrapper
接著要安裝 [virtualenv](http://virtualenv.readthedocs.org/en/latest/virtualenv.html) & [virtualenvwrapper](http://virtualenvwrapper.readthedocs.org/en/latest/install.html)
因為每個專案開發所需的環境可能不盡相同，virtualenvwrapper 幫助我們管理這些開發環境。

    $ pip install virtualenv
    $ pip install virtualenvwrapper

安裝 virtualenvwrapper 之後別忘了更改 ~./bash_profile，參考[這裏](http://virtualenvwrapper.readthedocs.org/en/latest/install.html#shell-startup-file)或以下三行
```
# Add to ~/.bash_profile and mkdir $HOME/.virtualenvs
export WORKON_HOME=$HOME/.virtualenvs
export PROJECT_HOME=$HOME/Devel
source /usr/local/bin/virtualenvwrapper.sh
```
更改完 ~/.bash_profile 後必須重新開 Termial 或者下以下指令才會生效!
```
    $ source ~/.bash_profile
```
> 詳細的 virtualenvwrapper 使用方法參考它的[文件](http://virtualenvwrapper.readthedocs.org/en/latest/)

#### 基本 virtualenvwrapper 使用
建立 virtualenv
```
$ mkvirtualenv <virtualenv-name>
```
刪除 virtualenvs
```
$ rmvirtualenv <virtualenv-name>
```
離開現在的 virtualenvs
```
$ deactivate
```
接著若您創立了一個虛擬環境，就可用以下指令進入該虛擬環境，然後再安裝 Django

    $ workon <virtualenv-name>
    (virtualenv-name)$ pip instal django == 1.5 # install Django 1.5 for example.

此時開啟 Pycharm 並開啟我的 Django 專案資料夾，並設定要用哪一個 Python intepreter
(你可能會有很多個在你電腦裡，這裡我選擇的是我產生的虛擬環境下的 Python)
這時 Pycharm 會提醒你有哪些尚未安裝的 Python Packages，但是你的 Project 有用到的，只要點 install，Pycharm就會幫忙你安裝好這些 Packages，這真是ㄧ個非常方便又懶人的功能... 或者你也不需要靠這功能，可以用以下指令將你有使用的 Packages 輸出成一個 requirement.txt 檔(裡面會是你 dependencies 的 list.)

    $ pip freeze > requirements.txt

再下指令即可一次安裝完所有 Packages，就不需要一個一個 install packages 了。

    $ pip install -r requirements.txt

做到這邊，基本上已經建置好當初好所有 Django Project 所需要的 dependencies.
所以下以下指令應該便能夠啟動簡單的 Server，來 run 我們的網站。

    (virtualenv-name)$ python manage.py runserver

接著因為我使用 Google Apache Engine 來 Hosting Django 的專案所以必須安裝下面兩支程式。
1. [**Google Cloud SDK**](https://cloud.google.com/sdk/)
2. [**Google AppEngineLauncher**](https://cloud.google.com/appengine/downloads)  

>安裝完 Google AppEngineLauncher 後，記得開啟該程式，他會問你是否 symlinks，記得要。

接著必須產生 Google 的 oauth 2.0 credential，才可在本地的 Terminal 上傳專案與管理 Cloud SQL 的資料庫 instance。

**＃如何產生 Google 的 oauth 2.0 之 credential file?**
可以使用以下指令來產生。

    $ gcloud config set account <google-mail>
    $ gcloud auth login # Get credentials for the tools in the Google Cloud SDK via a web flow.

或直接在 Terminal 下 syncdb 指令，但你必須先在 **my-django-project/setting.py** 裡面設定好與 Google Cloud SQL 內的資料庫 instance 做連結的程式碼，下面兩種方式。

###1.使用 django-appengine-toolkit packages 作輔助

setting.py 內的資料庫設定

    import appengine_toolkit
    import os
    APPENGINE_TOOLKIT = {
      'APP_YAML': os.path.join(BASE_DIR, 'app.yaml'),
    }
    DATABASES = {'default':appengine_toolkit.config(),}

app.yaml 也要別忘了要設定

    env_variables:
        DJANGO_SETTINGS_MODULE: '<project_name>.settings'
        DATABASE_URL: 'mysql://root@<app_id_of_gae>:<database_instance>/<database_name>'


###2.直接設定不透過 django-appengine-toolkit 與 Cloud SQL 做連結

    DATABASES = {
      'default': {
        'ENGINE': 'google.appengine.ext.django.backends.rdbms',
        'INSTANCE': '<project-id>:<database-instance>',
        'NAME': '<your-database-name>',
        'USER': 'root',
      }
    }

設定完與 Cloud SQL 的 instnace 連結後，下以下指令

    $ python manage.py syncdb # 此時會跳出網頁要你登入你的 Google 帳號

即可於 ~/ 底下產生 .googlesql_oauth2.dat 檔，便可使用以下指令 deploy 你的專案
> 若已經存在 .googlesql_oauth2.dat，但是產生 InternalError: (0, u'End user Google Account not authorized.') 的錯誤，可以刪除原有的 .googlesql_oauth2.dat，再重新下 syncdb 指令產生一次

    $ appcfg --oauth2 update . // 上傳專案至 GAE

**# 如何與 Cloud SQL 做連結，直接透過 Termial access cloud sql 內的資料庫？**  
需做 ~/.bash_profile 的設定

    export GAE="/usr/local/google_appengine"
    export PYTHONPATH="$PYTHONPATH:$GAE:$GAE/lib/dango-1.5"
    export PATH=${PATH}:$GAE/lib/django-1.5/django/bin/
    export DATABASE_URL='rdbms://root@<app_id_of_gae>:<database_instance>/<database_name>'

透過以下指令進入 cloud sql 的 mysql>

    $ mysql --host=instance-IP --user=user-name --password

**#如何升級 OS X 預設所帶的 GCC？**
使用 Macport...

### 讓 Terminal 與 vim 不再那麼色彩單調
更改 ~/.vimrc 讓你看程式不再黑黑一片。

    filetype plugin indent on
    syntax on

更改 ~/.bash_profile 讓 ls 指令可用顏色區分出不同檔案

    # Adding color to the shell.
    export CLICOLOR=1
    export LSCOLORS=gxBxhxDxfxhxhxhxhxcxcx
    alias ls='ls -G'

## 二、一些用到的實用小工具
     1. Source Tree - 一目瞭然地看自己 project 版本管理狀況。
     2. iTerm2 - 取代原本 OS X 的終端機。
     3. PDF Reader X - 好用的 PDF 檔案瀏覽程式，支援分頁。
     4. MPlayerX - OS X 上的播放軟體
     5. BetterTouchTool
     6. cDock
     7. CleanMyDrive
     8. XtraFinder
     9. DesktopUtility
    10. Battery Health
    11. The Unarchiver
    12. smcFanControl
    13. Alfred
    14. Dash

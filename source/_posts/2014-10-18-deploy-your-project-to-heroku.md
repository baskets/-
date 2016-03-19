---
layout: post
title: '#Note, Deploy Your Project to Heroku.'
date: 2014-10-18 15:18
comments: true
categories:
---
[Heroku](heroku.com) 支援上傳大部份可用來開發 web 的語言，主要是用 git 來做 Deploy，所以電腦裡必須裝有 git 這個版本控制程式。基本上上傳步驟都是依照 Heroku 官方的教學步驟進行，但是不免俗的還是會遇到一些問題...所以這篇文章是紀錄了發生什麼問題，以及如何解決。
<!--more-->

官方的上傳文件其實寫得非常清楚，首先必須先下載 Heroku toolbelt 來安裝。安裝完之後，就可以在 terminal 執行

    $ heroku login
    Enter your Heroku credentials.
    Email: <user>@gmail.com
    Password:
    Could not find an existing public key.
    Would you like to generate one? [Yn]
    Generating new SSH public key.
    Uploading ssh public key /Users/<username>/.ssh/id_rsa.pub

他會要你輸入帳號密碼，並且要選擇一個 key 上傳至 heroku，如果沒有 key 就依照他的步驟幫你建立，我的例子是我已經有 Github 和 Google Apache Engine 的 key 在我的 .ssh 資料夾之下，所以我選擇新建立一個給 heroku 的 key，透過以下指令來建立 key。

    $ ssh-keygen -t rsa
    Enter file in which to save the key (/Users/<username>/.ssh/id_rsa):

輸入 key 的檔案名即可建立，假設輸入的是 heroku，則應該會產生 heroku 和 heroku.pub 這兩個檔案在你現在位於的資料夾之下，再把它們倆個拉到 ~/.ssh/ 資料夾下即可。

接著再透過以下指令把我們剛剛建立的 key 上傳給 heroku

    $ heroku key:add <path to your key，這裏是 ~/.ssh/heroku.pub>

再輸入以下指令可以看看現在 heroku 上有我們剛剛上傳的 key

    $ heroku keys

若要刪除所有 heroku 上所儲存的 SSH keys，用下面的指令

    $ heroku keys:clear

上傳了 key 之後，就可以準備來上傳我們的 Project 了!
首先，先用 git 來開啟一個 local repository，來管控我們的專案，輸入以下指令。

    $ git init
    $ git add . # add all files to stage area.
    $ git commit -m 'init' # commit to local repository.

OK! 透過以下指令來直接在 heroku 上建立一個 App 來 host 我們的專案。

     $ heroku create
     Creating sharp-rain-871... done, stack is cedar
     http://sharp-rain-871.herokuapp.com/ | git@heroku.com:sharp-rain-871.git
     Git remote heroku added

他會隨機產生 App 的名字，譬如這邊就是 sharp-rain-871，若你想要自己取名可以到 [heroku dashboard](https://dashboard-next.heroku.com) 手動建立 App。

最後，在下 push 指令即可大功告成囉。

    $ git push heroku master

如果你沒遇到任何問題，就可以上 http://sharp-rain-871.herokuapp.com/ 去看看你的網站。
但我在 push 這邊遇到了這個問題
> 'Permission denied (publickey). fatal: Could not read from remote repository.'

認證 key 也上傳了，還是無法 push，完全依照官網步驟還是失敗，讓我百思不得其解。
最後在一個日本人的網站找到解法。
首先，在 ~/.ssh/ 下建立 config 檔

    $ vi ~/.ssh/config

config檔內容:

    Host heroku.com
    User git
    port 22
    Hostname heroku.com
    IdentityFile ~/.ssh/heroku <= 這是你的 key file，注意沒有 .pub。
    TCPKeepAlive yes
    IdentitiesOnly yes

設定完，就可以成功上傳了!
到底原因是什麼，我也還不太清楚，因為官方文件並沒提到這一步...
應該是官方這個[頁面](https://devcenter.heroku.com/articles/keys#fixing-problems-with-keys)所提到的問題，因為當在下 git 指令時，提供給 heroku 的並不是我上傳到 heroku 的 key，而是 git 的 key，兩個 key 不同，所以造成 Permission denied的問題，所以透過設定 config 檔來更改機制，根據 Host 來提供正確的 Key給 heroku，如此一來便不會 Permission denied 了。

以下是相同問題，但是不同的解法，或許可以幫到同樣發生此問題的人!
[Heroku 'Permission denied (publickey)](http://stackoverflow.com/questions/17626944/heroku-permission-denied-publickey-fatal-could-not-read-from-remote-reposito)

Thanks for [opamp.hatenablog](http://opamp.hatenablog.jp/entry/20110914/1316011399)

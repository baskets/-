---
layout: post
title: '#Note, Create django model'
date: 2015-03-04 00:50
comments: true
categories: Programming, Django
---
## Django model 建立注意事項
#### setting.py
記得在 INSTALLED_APPS 裡加入該 APP。

    INSTALLED_APPS = (..., 'your new app')

#### admin.py
記得註冊 Model，否則在管理頁面會看不到。

    from django.contrib import admin
    from allpay_payment.models import SomeModel
    admin.site.register(SomeModel)

#### syncdb
若該 APP 的 Model 要被 south 所管理，要輸入以下指令(for Django < 1.6)

    python manage.py schemamigration <app-name> --initial
    python manage.py schemamigration <app-name> --auto
    python manage.py migrate <app-name>

若否，則可直接

    python manage.py syncdb

#### 若是在 localhost
記得重新啟動 server，否則管理頁面也會不變。

    python manage.py runserver

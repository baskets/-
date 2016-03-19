---
layout: post
title: '#Note, South migration flow record.'
date: 2015-03-04 00:37
comments: true
categories: programming, Django
---
##South
####What to do when database schema of Django application changed.

####Install the south package

    pip install south

So that you can manage your model by south.
If a Django application is not managed by south and not be syncdb in advance,  
we should do --initial.

    ./manage.py schemamigration <AppName> --initial

But If you have already syncdb a application, there is another way to make  
the app be managed by the south. Look up the documentation of south.  

<!--more-->

####Assure the app has already been initialized by south.
if you changed your schema of database in codes, you can apply the commands  
below to change the schema.

    ./manage.py schemamigration <AppName> --auto
    ./manage.py migrate <AppName>

By applying these two commands, the schema of database has been changed.  
[South Document.](http://south.readthedocs.org/en/latest/tutorial/part1.html#changing-the-model)
## Others...

#### Manage Your Model in the Django build-in admin site.
If you want to manage the model in admin site, you should register them in  
the admin.py. like this.

    from django.contrib import admin
    # Register your models here.
    from <AppName>.models import SomeModel
    admin.site.register(SomeModel)

####How to Connect to Google's sql cloud
    mysql --host=instance-IP --user=user-name --password

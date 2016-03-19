---
layout: post
title: '#Note, The priority of static files when the static finder traverse the /static/ folder.'
date: 2015-06-05 23:16
comments: true
categories: programming, Django
---
In our template, we'll declare something like below code snippet to load static files.

    {/% load staticfiles /%}
    {/% static 'js/some.js' /%}

Your Django project structure may like this, so what is the priority of these some.js when the static finder try to find them?

    site
    |__ a_app
        |__ static
            |__ js
                |__ some.js

    |__ b_app
        |__ static
            |__ js
                |__ some.js

    |__ static
        |__ js
              |__ some.js

It is depended on the variable 'INSTALED_APPS' declared in site/settings.py.

     INSTALLED_APPS = (
                     'a_app',
                     'b_app',
                     )

The priority of these some.js files in different directories (but the same structure 'static/js') will be...

*site/static/js/some.js --->  site/a_app/static/js/some.js ---> site/b_app/static/js/some.js*

So the site/static/js/some.js will be found by finder first.

But if we change the order of apps in 'INSTALLED_APPS':

     INSTALLED_APPS = (
                     'b_app',
                     'a_app',
                     )

The priority will be changed to...

*site/static/js/some.js --->  site/b_app/static/js/some.js ---> site/a_app/static/js/some.js*    
This is a note of experiment done by my own, not for sure the mechanism is right.
I'll check out the documentation of Django for correct mechanism :-)

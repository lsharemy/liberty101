title: Ubuntu 10.04 + Heroku + Django
tags:
  - Django
  - heroku
  - Ubuntu
id: 1526
categories:
  - wordpress
  - index.php
  - csit
date: 2012-06-26 00:37:19
---

官方教程：[https://devcenter.heroku.com/articles/django](https://devcenter.heroku.com/articles/django)

到这一步：
<pre lang="term">$ pip install Django psycopg2 dj-database-url</pre>
会出错，是因为缺少头文件，解决方法在这里：[http://stackoverflow.com/questions/5420789/how-to-install-psycopg2-with-pip-on-python](http://stackoverflow.com/questions/5420789/how-to-install-psycopg2-with-pip-on-python)

因为换成了2.7的python，要装2.7的开发包，python2.7-dev<!--more-->

在settings.py里添加
<pre lang="python">import dj_database_url
DATABASES = {'default': dj_database_url.config(default='postgres://localhost')}</pre>
这里不太明白，settings.py里面已经有一个DATABASES的定义了，这个又是什么意思呢。

然后，执行到python manage.py syncdb的时候，总是出错

本以为是2.6的问题，结果换了2.7，还是一样= =

今天是搞不出来了，明天继续。

-----------------------------------------------------------------------------------

好吧，再学习完Django的教程：[https://docs.djangoproject.com/en/1.4/intro/](https://docs.djangoproject.com/en/1.4/intro/)之后，终于搞明白了

dj_database_url是一个让你用url的方式来配置数据库的工具，而postgres://localhost是服务器端可用的数据库链接，所以上传后执行heroku run python manage.py syncdb是没有问题的，因为是在服务器端执行的

但要执行python manage.py syncdb同步本地数据库就会出问题，因为在本地使用postgres的话，需要安装并配置postgres，而且还需要更改数据url为postgres://username:password@localhost:5432/mydb这样的形式

教程最后的使用celery也没有测试成功，因为好像要收费？

最后的页面在这里：<del>[http://pure-flower-8857.herokuapp.com/](http://pure-flower-8857.herokuapp.com/)</del>

[http://django-lmy.herokuapp.com/](http://django-lmy.herokuapp.com/)

&nbsp;
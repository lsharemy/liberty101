title: Ubuntu 10.04 + Django 1.4
tags:
  - Django
  - heroku
  - Ubuntu
id: 1536
categories:
  - wordpress
  - index.php
  - csit
date: 2012-06-26 22:33:43
---

好吧，昨天被Heroku的Django教程搞郁闷了

今天来补课

教材在这里：[https://docs.djangoproject.com/en/1.4/](https://docs.djangoproject.com/en/1.4/)

## First steps

*   **From scratch:** [_Overview_](https://docs.djangoproject.com/en/1.4/intro/overview/) | [_Installation_](https://docs.djangoproject.com/en/1.4/intro/install/)
*   **Tutorial:** [_Part 1_](https://docs.djangoproject.com/en/1.4/intro/tutorial01/) | [_Part 2_](https://docs.djangoproject.com/en/1.4/intro/tutorial02/) | [_Part 3_](https://docs.djangoproject.com/en/1.4/intro/tutorial03/) | [_Part 4_](https://docs.djangoproject.com/en/1.4/intro/tutorial04/)
过了下文档的这一部分，其中Overview是Django的一个概述，Installation讲了如何安装，之后的Tutorial分4部分来讲如何进行Django的使用

<span style="color: #ff0000;">第一部分</span>

这部分讲了如何使用django-admin.py startproject命令创建一个Django项目，并介绍了项目中各个文件的作用<!--more-->

还讲了如何在本地运行Django的一个用于开发的服务器，python manage.py runserver

还介绍了如何设置数据库，以及python manage.py syncdb命令的使用

然后介绍了如何用python manage.py startapp来创建一个APP

并创建该APP的model，并通过在INSTALLED_APPS中添加该APP来激活它，然后直接执行python manage.py syncdb，Django就会自动根据你的model来创建数据库中的表于之对应

还介绍了如何使用python manage.py shell命来来玩Django提供的API

<span style="color: #ff0000;">第二部分</span>

这部分教你怎么创建管理员用的后台

只要在工程的<tt>urls.py文件中，取消其中3行的注释就好了，噢，还有<tt>INSTALLED_APPS中要把"django.contrib.admin"的注释取消</tt></tt>

<tt></tt>不过这个时候，还不能对自己定义的model中的数据进行管理，不过通过admin.site.register()注册以下就好啦

管理界面相当强大，还可以自定义数据的显示，以及页面的风格等等

<span style="color: #ff0000;">第三部分</span>

前两部分已经把model建好了，接下来就是view啦

首先要设计url的结构，这个定义了怎么样的url访问由哪个view来处理

然后介绍了如何来写View，还教你用模板来将页面设计和Python代码分开

还介绍了render_to_response()、get_object_or_404()等等的使用

最后还讲了如何对URLconfs进行去耦，从而使得APP可以“即插即用”

<span style="color: #ff0000;">第四部分</span>

这部分介绍了如何进行表单处理，并讲了如何用generic views来减少代码

---------------------------------------------------------------------------------

教程中提到，存放模板的目录由TEMPLATE_DIRS来设置，而且必须使用绝对路径，这也就限制了项目的移动，不过也是有办法的：[http://stackoverflow.com/questions/4562252/django-how-to-deal-with-the-paths-in-settings-py-on-collaborative-projects](http://stackoverflow.com/questions/4562252/django-how-to-deal-with-the-paths-in-settings-py-on-collaborative-projects)

-----------------------------------------------------------------------------------

最后把这个教程的代码部署到了Heroku上，忽忽

这里是地址：<del>[http://warm-snow-5315.herokuapp.com/polls/](http://warm-snow-5315.herokuapp.com/polls/)</del>，（链接更新到下面）

--------------------------------------------------------------------------------

就是有一个地方没想明白，我用的是sqlite数据库，为什么服务器提示以下错误：

<span style="color: #ff0000;">Error: No module named psycopg2.extensions</span>

在requirements.txt中添加了psycopg2和dj-database-url，竟然就通过了

----------------------------------------------------------

学会了改名，把链接更新下：[http://vote-lmy.herokuapp.com/polls/](http://vote-lmy.herokuapp.com/polls/)
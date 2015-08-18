title: 从Heroku中导出导入sqlite数据库文件
tags:
  - heroku
id: 1563
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-06 16:02:50
---

教程在这里：[https://devcenter.heroku.com/articles/taps](https://devcenter.heroku.com/articles/taps)

不过执行heroku db:pull命令的时候出现以下错误：

Taps Load Error: no such file to load -- sqlite3<!--more-->

这篇文章给出了解决办法：[https://getsatisfaction.com/railstutorial/topics/heroku_db_push_issue_solved](https://getsatisfaction.com/railstutorial/topics/heroku_db_push_issue_solved)

再然后又出现了以下错误：

extconf.rb:12:in `require': no such file to load -- mkmf (LoadError)

这篇文章给除了解决方法：[http://iamdaiyuan.iteye.com/blog/106112](http://iamdaiyuan.iteye.com/blog/106112)

再然后遇到以下错误：

! Invalid database url

正在找解决方法= =

----------------------------------------------------------

好吧，就是链接错了改成heroku db:pull sqlite:///home/lmy/django/mysite/word_db就把远程的数据库文件下载到本地的/home/lmy/django/mysite/word_db了

不过数据库中的DateTimeField字段的值会丢失，现在还没找到原因，至少可以pull啦;-D

--------------------------------------------------------------

接下来开始push

heroku db:push sqlite:///home/lmy/django/mysite/word_db

时间字段木有丢失，不错不错

---------------------------------

很可能是因为用了和时区相关的时间的原因，这里貌似有说到这个问题：[http://stackoverflow.com/questions/4530069/python-how-to-get-a-value-of-datetime-today-that-is-timezone-aware/4530166#4530166](http://stackoverflow.com/questions/4530069/python-how-to-get-a-value-of-datetime-today-that-is-timezone-aware/4530166#4530166)

不过先不去研究了

准备把word-ninja先部署起来
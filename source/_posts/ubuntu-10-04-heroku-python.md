title: Ubuntu 10.04 + Heroku + Python
tags:
  - heroku
  - Python
  - Ubuntu
id: 1519
categories:
  - wordpress
  - index.php
  - csit
date: 2012-06-25 20:23:09
---

在Windows是部署Heroku碰壁了以后，决定用Ubuntu来玩

官方的quick start：[https://devcenter.heroku.com/articles/quickstart](https://devcenter.heroku.com/articles/quickstart)

其中的第二步是安装工具包：[https://toolbelt.heroku.com/](https://toolbelt.heroku.com/)

刚开始用wget -qO- [https://toolbelt.heroku.com/install.sh](https://toolbelt.heroku.com/install.sh) | sh这个命令装的，但部署的时候出现各种问题。<!--more-->

后来找到了这篇博客：[http://blog.lzhaohao.info/archive/deploy-a-web-py-application-on-heroku/](http://blog.lzhaohao.info/archive/deploy-a-web-py-application-on-heroku/)

决定用[RubyGems](http://rubygems.org/)来试试看

Ubuntu 10.04的源中的gem是1.3.5版本的，用它来安装heroku和foreman的时候，会出错，需要更新

更新教程见官方：[http://rubygems.org/pages/download](http://rubygems.org/pages/download)

需要注意的地方就是update_rubygems命令需要到/var/lib/gems/1.8/bin/目录下去执行

更新好之后就可以gem install heroku foreman啦

再去Heroku官网看Python的教程：[https://devcenter.heroku.com/articles/python](https://devcenter.heroku.com/articles/python)

按着流程走，之前出现的很多错误都不见了，忽忽，成功~

<del>[http://afternoon-winter-5386.herokuapp.com/](http://afternoon-winter-5386.herokuapp.com/)</del>，

---------------------------------------------------------------------

用命令行的方式改名也玩一下

heroku apps:rename helloworld-lmy

链接更新为：[http://helloworld-lmy.herokuapp.com/](http://helloworld-lmy.herokuapp.com/)
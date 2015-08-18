title: Ubuntu 10.04 + Python2.7
tags:
  - heroku
  - Python
  - Ubuntu
id: 1514
categories:
  - wordpress
  - index.php
  - csit
date: 2012-06-25 18:01:48
---

为了玩heroku，要装个2.7的Python

在这里发现了个比较方便的方法：[http://askubuntu.com/questions/101591/install-python-2-7-2-on-ubuntu-10-04-64-bit](http://askubuntu.com/questions/101591/install-python-2-7-2-on-ubuntu-10-04-64-bit)

就是通过增加ppa源的方法

    sudo add-apt-repository ppa:fkrull/deadsnakes`
    `sudo apt-get update`
    `sudo apt-get install python2.7

-----------------------------------------------------

装了2.7，发现用的还是原来的2.6，于是想，把2.6给remove掉就可以了吧，结果一remove，系统就坏了，重装！！！
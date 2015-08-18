title: Python + Distribute + Pip + Virtualenv
tags:
  - Python
id: 1528
categories:
  - wordpress
  - index.php
  - csit
date: 2012-06-26 11:55:34
---

教程在这里：[http://docs.python-guide.org/en/latest/starting/install/linux/](http://docs.python-guide.org/en/latest/starting/install/linux/)

Python就是Python

Distribute是用来安装Python的软件包的<!--more-->

安装好Distribute之后会多出来一个easy_install命令

不过easy_install被认为是过时的，它的代替者就是Pip

于是用easy_install安装一个Pip，命令为easy_install pip

Virtualenv则是提供一个虚拟的Python环境

安装命令：pip install virtualenv

之后，用virtualenv --distribute venv来创建一个虚拟的Python环境，用source venv/bin/activate来切换到这个环境中。
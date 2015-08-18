title: Google App Engine - Python
tags:
  - Google App Engine
  - Python
id: 1479
categories:
  - wordpress
  - index.php
  - default
date: 2012-06-01 21:14:47
---

研究下GAE，顺便再学学Python。

教程在这里：[https://developers.google.com/appengine/docs/python/gettingstarted/](https://developers.google.com/appengine/docs/python/gettingstarted/)

下好了[App Engine SDK](https://developers.google.com/appengine/downloads)，安装好后运行Google App Engine Launcher，出现以下警告：

<span style="color: #ff0000;">Warning: Prerequisites for App Engine development are missing!<!--more--></span>

<span style="color: #ff0000;">A valid python binary must be available. In addition,the App Engine SDK must be installed. Here are the current values we found:</span>

<span style="color: #ff0000;">python = None</span>

<span style="color: #ff0000;">App Engine SDK root = F:\Google\google_appengine</span>

<span style="color: #ff0000;">Please install the missing pieces and restart the launcher.If these are installed but the Launcher failed to find them, you can configure their location by editing Launcher preferences.</span>

<span style="color: #ff0000;">The Launcher preferences can be modified by selecting Edit &gt; Preferences</span>

意思是没有找到Python，不过根据提示，在Edit-&gt;Preferences下设置下Python 2.5的路径(F:\Python25\python.exe)就好了。

跟着教程走了一遍，发现部署完访问不能，这也要翻墙？T_T
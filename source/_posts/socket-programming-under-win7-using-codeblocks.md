title: 'win7下用code::blocks编写socket程序设置'
tags:
  - 'code::blocks'
  - socket
id: 1422
categories:
  - wordpress
  - index.php
  - csit
date: 2012-05-04 11:35:29
---

网上各种设置方法，有加ws2_32.lib库的，有加头文件的

最后发现只要加个链接选项-lws2_32就OK了<!--more-->

[http://who-know.com/windows-c-ide-c-socket-programming/](http://who-know.com/windows-c-ide-c-socket-programming/)，

出错的话用WSAGetLastError()打印错误信息
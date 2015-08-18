title: Ubuntu 10.04 + VirtualBox 4.1.18 + Win7
tags:
  - Ubuntu
  - VirtualBox
id: 1607
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-12 12:15:02
---

以前总是Win7 + VirtualBox + Ubuntu，现在反过来，这样应该就会多用Ubuntu一点了，忽忽

装好后，先装Guest Additions了，然后试了下USB，把用户加入到vboxusers组：sudo usermod -a -G vboxusers username，然后勾选相应的U盘，结果在Win7里提示驱动安装失败，不晓得什么原因

不过Shared Folders设置很方便，这样不用U盘也行啦，反正宿主机和虚拟机能方便的传输文件就好。
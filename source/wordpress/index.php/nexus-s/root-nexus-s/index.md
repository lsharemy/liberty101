title: root
id: 1099
date: 2012-01-20 16:28:44
---

所需工具：su-2_3_6_1-ef-signed_153038_140350.zip

把su-2_3_6_1-ef-signed_153038_140350.zip放到sdcard根目录下

[进入到ClockworkMod Recovery](http://lsharemy.com/wordpress/index.php/nexus-s/flashing-clockworkmod-recovery/)之后

选择install ZIP from sdcard，再选choose zip from sdcard，然后选择放入的su-2_3_6_1-ef-signed_153038_140350.zip，确认安装，就会开始安装了

其实就是用压缩包里的su代替了系统里的bin/su和xbin/su，并安装了个一个Superuser.apk

完成够，返回到ClockworkMod Recovery初始界面

选择reboot system now重启手机

---------------------------------

并没有出现“授权管理”程序，自己手动安装的（apk在zip包里可以找到）

---------------------------------

发现还没有获得root权限- -，失败

------------------------------------

好吧

按这个教程再试一遍

[http://nexusshacks.com/nexus-s-hacks/how-to-root-nexus-s/](http://nexusshacks.com/nexus-s-hacks/how-to-root-nexus-s/)

------------------------

使用该教程的su-2.3.6.1-ef-signed.zip，其他不变

终于出现“授权管理”程序了，用豌豆荚打开，该程序在系统应用下面

成功获得root权限，忽忽

-----------------------
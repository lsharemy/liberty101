title: Nexus S刷4.1.1 Jelly Bean
id: 1826
date: 2012-07-24 15:50:53
---

官方ROM下载地址（经过确认，这个是升级包，不是FULL ROM，所以只能从4.0.4刷过来）：

http://android.clients.google.com/packages/ota/google_crespo/9ZGgDXDi.zip

使用官方原生Recovery来刷

1.用【音量向上键+电源键】进入fastboot

2.然后选择recovery并按电源键进入

3.看到一个感叹号机器人，先【电源键】按住，再按【音量+键】，弹出菜单

4.进行双wipe（wipe data/factory reset和wipe cache partition）

5.apply update from /sdcard，选择从前面下载的.zip包（前面忘了说了，要把下载的zip先放到sdcard下面）

6.reboot system now

这样就好啦

-------------------------------------------

补充：这个原来是升级包，需要从4.0.4升级。

附一个好的站点：[http://www.randomphantasmagoria.com/firmware/nexus-s/](http://www.randomphantasmagoria.com/firmware/nexus-s/)，包含了所有可用的ROM。

-------------------------------------------

发现这几天GPS搜不到了，不晓得是4.1.1的缘故还是都有这个问题= =

[http://forum.xda-developers.com/showthread.php?t=1239713](http://forum.xda-developers.com/showthread.php?t=1239713)，

[http://code.google.com/p/android/issues/detail?id=35335](http://code.google.com/p/android/issues/detail?id=35335)，

[http://bbs.gfan.com/android-4619786-1-1.html](http://bbs.gfan.com/android-4619786-1-1.html)，

以上是相关帖子，不过照着最后一个帖子修改gps.conf并重启之后，还是木有解决= =

-----------------------------------------

准备又回去4.0.4看看，而且想刷个韩版的试试

------------

好吧，一样定位不能，不折腾了，先等等官方的解释
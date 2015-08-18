title: 'ROOT Nexus S with 4.1.1 Jelly Bean '
id: 1828
date: 2012-07-24 17:04:28
---

背景：自从以前刷4.0.3并root了之后，进行了一次OTA升级，升级好之后，superuser这个应用还在，但是root权限已经没有了，这次升级了4.1.1，superuser竟然还在。于是又下了个全新的官方4.1.1的ROM。刷进去之后，竟然superuser还在= =。好吧，再来root一遍试试！（好吧，原来OTA升级软件神马的本来就会都在的，我傻了）

1.先确定有没装好驱动，装官方的好像不行，我每次都去这里下载：[http://www.anzhuo.cn/thread-144743-1-1.html](http://www.anzhuo.cn/thread-144743-1-1.html)

2.开机时，用【音量向上键+电源键】进入fastboot

3.这个时候在电脑的cmd中输入，fastboot.exe flash recovery recovery.img，这里需要2个东西，一个是fastboot.exe，这个在Android的SDK中可以找到，也可以去步骤1中给出的链接里去下载。还有一个就是recovery.img，我这里使用的是ClockworkMod 6.0.1.0的Touch版本。在这里下载：[http://www.clockworkmod.com/rommanager](http://www.clockworkmod.com/rommanager)

4.然后马上选择Recovery进入

5.install zip from sdcard

6.choose zip from sdcard

7.选择Superuser-3.1.3-arm-signed.zip，这个可以从这里下载：[http://bbs.gfan.com/android-4657260-1-1.html](http://bbs.gfan.com/android-4657260-1-1.html)

8.然后再在主菜单里的adanve里fix permissions一下（这步可能不需要，不过我还是做了遍）

9.重启

10.打开superuser，点“点击此处检查更新"，然后再点更新

DONE
title: 解锁bootloader
id: 1076
date: 2012-01-20 15:11:03
---

官方也有教程：

[http://source.android.com/source/building-devices.html](http://source.android.com/source/building-devices.html)

上面页面的Unlocking the bootloader小节。

命令：<span style="color: #008000;">fastboot oem unlock</span>

fastboot也是官方提供的工具，在SDK中没有包含，也可以从源代码中自己编译。网上也有很多编译好的可执行文件下载。

---------------------------

不过这里出现 &lt; waiting for device &gt;就停住了，查了下是驱动的问题。我装的是官方的驱动，在手机开机状态下可以识别，不过好像进入fastboot模式就识别不了了。

在[http://www.anzhuo.cn/thread-144743-1-1.html](http://www.anzhuo.cn/thread-144743-1-1.html)下了驱动

再试

-------------------------------

成功，弹出一个确认界面，选YES，就成功UNLOCK了（注意，会删除所有用户数据）

这时，bootloader的状态就变为：

<span style="color: #ff0000;">LOCK STATE - UNLOCKED</span>

------------------------

这时候再敲入<span style="color: #008000;">fastboot oem lock</span>

又变回：

LOCK STATE - LOCKED

------------------------

解锁后重启看了一下，又看到了韩文，装的应用程序都不见了，sdcard下的数据也都没了，不过系统版本还是2.3.4，没有回到出厂的2.3.3系统。
title: 'Hello, Android 4.0 and Galaxy Nexus | Failed to install HelloAndroid.apk on device'
tags:
  - Android
id: 344
categories:
  - wordpress
  - index.php
  - csit
date: 2011-10-21 14:02:38
---

![](http://localhostr.com/file/sAKPhqJ/HelloAndroid2.jpg "HelloAndroid2")

话说前天<span style="color: #f18200;">Android 4.0</span>和<span style="color: #f18200;">Galaxy Nexus</span>的发布会还是让我小激动的，再加上网上<span style="color: #f18200;">Galaxy Nexus</span>只要<span style="color: #ff0000;">299</span>刀的传言更是让我有了换手机的冲动。结果发布会上木有提钱的事情，随后，有消息称，谷歌<span style="color: #f18200;">GALAXY Nexus</span>已经在英国开始接受预定，其含税价格为<span style="color: #ff0000;">514.80</span>英镑！（失落）

<!--more-->

好吧，还是继续用我的破黑莓。不过还是想学下<span style="color: #f18200;">Android</span>下面的开发，因为前两天去Hiall搜索视频编码的实习信息，看到有个招A<span style="color: #f18200;">ndroid</span>视频编码的，刚好可以跟视频编码结合到一起，于是很想试一下。（终于找到个合适的借口）

先下一个<span style="color: #f18200;">Android SDK Starter Package</span> [http://developer.android.com/sdk/index.html](http://developer.android.com/sdk/index.html)

然后follow这里[http://developer.android.com/sdk/installing.html](http://developer.android.com/sdk/installing.html)的教程即可，大概步骤如下：

安装好<span style="color: #f18200;">Android SDK Starter</span>之后（需要先安装有<span style="color: #f18200;">JDK</span>），用它来下载<span style="color: #f18200;">Android platform</span>，这里会自动选择最新的<span style="color: #ff0000;">4.0</span>平台，只要点击**Install**即可。

然后么，装个<span style="color: #f18200;">Eclipse</span>，<span style="color: #f18200;">Classic</span>版本就ok

再就是装<span style="color: #f18200;">Eclipse</span>的<span style="color: #f18200;">ADT</span>插件，教程在这里[http://developer.android.com/sdk/eclipse-adt.html#installing](http://developer.android.com/sdk/eclipse-adt.html#installing)

都完成之后，重启<span style="color: #f18200;">Eclipse</span>，设置好<span style="color: #f18200;">Android</span>的<span style="color: #f18200;">SDK</span>目录，就可以开始<span style="color: #ff0000;">Helloworld</span>啦

tutorial官方也有[http://developer.android.com/resources/tutorials/hello-world.html](http://developer.android.com/resources/tutorials/hello-world.html)

一步步按教程操作，run！终于还是遇到了问题，首先是Android模拟器的启动需要很长时间，需要耐心等待，然后出现了一下错误：
<pre>[2011-10-21 11:29:26 - HelloAndroid] Failed to install HelloAndroid.apk on device 'emulator-5556!
[2011-10-21 11:29:26 - HelloAndroid] (null)
[2011-10-21 11:29:26 - HelloAndroid] Failed to install HelloAndroid.apk on device 'emulator-5556': EOF
[2011-10-21 11:29:26 - HelloAndroid] com.android.ddmlib.InstallException: EOF
[2011-10-21 11:29:26 - HelloAndroid] Launch canceled!</pre>
原来这个错误是没有给模拟器添加内存卡造成的，于是加个512MB的内存卡，再run，就ok啦，截个图晒一晒：

![](http://localhostr.com/file/krgrLJV/HelloAndroid.jpg "HelloAndroid")

嘿，买不起<span style="color: #f18200;">Galaxy Nexus</span>，就拿模拟器先过把瘾了。
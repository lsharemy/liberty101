title: '在实际设备（HTC HERO@sprint）上运行Android程序 | Android Develop - Using Hardware Devices'
tags:
  - Android
id: 367
categories:
  - wordpress
  - index.php
  - csit
date: 2011-11-04 20:56:28
---

![](http://localhostr.com/file/8RFFDVW/HelloAndroid_htc_1.jpg "HelloAndroid_htc_1")

今天开始读<span style="color: #f18200;">Android基础教程（第3版）</span>，跑图书馆呆了一个下午。

晚上回实验室开始动手。

这里有详细的步骤：[http://developer.android.com/guide/developing/device.html](http://developer.android.com/guide/developing/device.html)

<!--more-->

首先需要在<span style="color: #f18200;">AndroidManifest.xml</span>中的<span style="color: #f18200;">&lt;application&gt;</span>元素下增加这一行：<span style="color: #f18200;">android:debuggable="true"</span>

然后，在手机 设置 应用程序 中允许未知源软件的安装，并打开USB调试开关

最后，也是最终重要的，需要下载一个<span style="color: #f18200;">usb driver</span>来让<span style="color: #f18200;">adb</span>能识别你的设备，而这个驱动则是厂商相关的，需要根据不同手机下载不同的驱动。

我的是<span style="color: #f18200;">HTC HERO@sprint</span>，下载链接为[http://www.htc.com/us/support/hero-sprint/downloads/](http://www.htc.com/us/support/hero-sprint/downloads/)

不过并没有找到什么<span style="color: #f18200;">usb driver</span>的驱动，后来了解到<span style="color: #f18200;">HTC SYNC</span>中包含了这个驱动，所以下载<span style="color: #f18200;">HTC SYNC</span>并安装，也就安装好了<span style="color: #f18200;">usb driver</span>。

这个时候，连接手机，用<span style="color: #f18200;">adb devices</span>命令就可以检查是否成功识别的设备了。

然后在<span style="color: #f18200;">Eclipse</span>中运行<span style="color: #f18200;">Android</span>应用程序，在设备选择的时候可以看到连接的手机，选择它，程序便会在手机上加载并运行了。

以下是截图：

![](http://localhostr.com/file/oV2RG0j/HelloAndroid_htc_2.jpg "HelloAndroid_htc_2")

大功告成，忽忽~
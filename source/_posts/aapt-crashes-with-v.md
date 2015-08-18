title: "Error generating final archive: java.io.FileNotFoundException: ×××\bin\resources.ap_ does not exist & aapt.exe停止工作"
tags:
  - Android
id: 429
categories:
  - wordpress
  - index.php
  - csit
date: 2011-11-07 17:18:16
---

![](http://i.minus.com/ibxgXD1venQHoK.jpg "aapt")

解决个错误，记录下。

错误如下：
<span style="color: #ff0000;">Description Resource Path Location Type</span>
<span style="color: #ff0000;"> Error generating final archive: java.io.FileNotFoundException: C:\Users\lmy\Desktop\android\HelloAndroid\bin\resources.ap_ does not exist HelloAndroid Unknown Android Packaging Problem</span>

<!--more-->

每次<span style="color: #f18200;">aapt.exe</span>停止工作之后就会出现这个错误。

<span style="color: #f18200;">aapt.exe</span>为什么会停止工作呢，原来是选项-v弄得，在<span style="color: #f18200;">eclipse</span>中<span style="color: #f18200;">Window-&gt;Preferences-&gt;Android-&gt;Built</span>下的<span style="color: #f18200;">Built output</span>，选择了<span style="color: #f18200;">Verbose</span>，才导致以上的错误，还是继续<span style="color: #f18200;">silent</span>吧。

解决方法是从这看到的：[http://www.eoeandroid.com/forum.php?mod=viewthread&amp;tid=103417](http://www.eoeandroid.com/forum.php?mod=viewthread&amp;tid=103417)

而真正的解决方法是一个google group的链接（[https://groups.google.com/group/android-developers/browse_thread/thread/9d48edaee8d2a66f/64faf61ce1dd6e14](http://groups.google.com/group/android-developers/browse_thread/thread/9d48edaee8d2a66f/64faf61ce1dd6e14))，在找不到翻墙工具的情况下，求助了TX工作的刀疤轮，一个截图果然很给力啊，忽忽：（<span style="color: #ff0000;">好吧，RZY的方法更给力，把https改成http就可以了！</span>）

![](http://i.minus.com/iRv1r9FL79U2L.jpg "honglun")
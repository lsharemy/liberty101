title: "ADB server didn't ACK | * failed to start daemon *"
tags:
  - Android
id: 457
categories:
  - wordpress
  - index.php
  - csit
date: 2011-11-09 18:34:11
---

出现以下错误：

<span style="color: #ff0000;">[2011-11-09 16:28:26 - adb]ADB server didn't ACK</span>
<span style="color: #ff0000;"> [2011-11-09 16:28:26 - adb]* failed to start daemon *</span>

解决方法：

关掉eclipse，杀死adb.exe进程，（关掉豌豆荚），重启eclipse。
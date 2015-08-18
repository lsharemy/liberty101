title: Android + animation + AsyncTask记录
tags:
  - Android
id: 1574
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-07 23:18:12
---

今天把启动界面做了下

一个是要完成数据库的插入，然后顺便研究了下Activity之间的切换动画

由于数据量很大，数据库的插入需要很长的时间，于是使用AsyncTask+进度条的方式

主要参考这<!--more-->
里：[http://grepcode.com/file/repository.grepcode.com/java/ext/com.google.android/android/2.1_r2/android/os/AsyncTask.java](http://grepcode.com/file/repository.grepcode.com/java/ext/com.google.android/android/2.1_r2/android/os/AsyncTask.java)

然后是修改默认的切换动画，使用这个函数：[overridePendingTransition](http://developer.android.com/reference/android/app/Activity.html#overridePendingTransition(int, int))

这里有篇好文：[http://www.anddev.org/novice-tutorials-f8/splash-fade-activity-animations-overridependingtransition-t9464.html](http://www.anddev.org/novice-tutorials-f8/splash-fade-activity-animations-overridependingtransition-t9464.html)
title: 'ERROR/AndroidRuntime(256):  android.content.ActivityNotFoundException: No Activity found to handle Intent'
tags:
  - Android
id: 451
categories:
  - wordpress
  - index.php
  - csit
date: 2011-11-08 20:33:36
---

在通过<span style="color: #f18200;">意图intent</span>来打开外部浏览器的程序中遇到了一下错误：

<span style="color: #ff0000;">11-08 09:29:16.616: ERROR/AndroidRuntime(256): android.content.ActivityNotFoundException: No Activity found to handle Intent { act=android.intent.action.VIEW dat= }</span>

结果发现是输入网址时，没有输<span style="color: #f18200;">http://</span>前缀造成的。记录下。

&nbsp;
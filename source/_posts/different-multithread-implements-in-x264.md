title: 'x264不同版本的多线程实现 | Different Multithread Implements in x264'
tags:
  - codec
  - H.264/AVC
  - x264
id: 184
categories:
  - wordpress
  - index.php
  - csit
date: 2011-07-18 10:45:20
---

[caption id="" align="aligncenter" width="329" caption="Intel vTune Amplifier XE 2011 - x264 4 threads"]![](http://localhostr.com/file/n8ZL7ZZ/x264.png "x264 Multithread")[/caption]

前段时间看了x264的多线程，总结一下，也方便一下研究x264的小朋友。

<!--more-->

x264用过两种实现多线程的方式，slice-based和frame-based。

早期的时候使用的是slice-based，后来改为frame-based，再后来，又重新引入slice-based（作为可选方式），用于低时延的情形。

多线程实现方式和版本的对应关系大概如下：

<span style="color: #ff6600;">20061215之前的版本使用的slice-based</span>

<span style="color: #ff6600;">20061216版本开始，改为frame-based</span>

<span style="color: #ff6600;">20101011版本又将slice-based方式作为一种可选选项重新引入</span>

我是怎么知道的？去<span style="color: #000000;">[ftp://ftp.videolan.org/pub/videolan/x264/snapshots/](ftp://ftp.videolan.org/pub/videolan/x264/snapshots/)</span>，下载一个包，打开doc\threads.txt看一下吧

不过里面讲的版本号r607、r1246和r1364我还不知道分别对应哪几个包，tell me if you know:-D

btw：20091006是最后一个包含build文件夹的版本，也就是最后一个兼容windows的版本。至少我还不知道如何在windows下编译最新的code。

&nbsp;
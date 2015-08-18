title: 编译最新版本x264
id: 803
date: 2011-12-22 20:39:04
---

目前最新版本为x264-snapshot-20111221-2245

官网在这里[http://www.videolan.org/developers/x264.html](http://www.videolan.org/developers/x264.html)

官网上有编译教程，也有支持论坛

编译过程主要的错误就是缺少yasm或者yasm的版本不够新

yasm的官网：[http://yasm.tortall.net/](http://yasm.tortall.net/)

去下载最新源码，编译安装即可，基本不会出现错误，只需要build-essential安装了就可以。

如果需要x264的静态库，则在configure的时候加上--enable-static选项，其他选项见./configure --help。
title: Ubuntu 10.04 编译Android源代码
tags:
  - Android
  - Ubuntu
id: 484
categories:
  - wordpress
  - index.php
  - csit
date: 2011-11-11 16:32:08
---

教程在这里：[Building the System](http://source.android.com/source/building.html)

不过之前还需要做一些工作：[Initializing a Build Environment](http://source.android.com/source/initializing.html)

<span style="color: #ff0000;">安装JDK：</span>

<!--more-->

<span style="color: #f18200;">sudo apt-get install sun-java6-jdk</span>

<span style="color: #ff0000;">安装依赖包：</span>

官方给出的是：

<span style="color: #f18200;">sudo apt-get install git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev libc6-dev lib32ncurses5-dev ia32-libs x11proto-core-dev libx11-dev lib32readline5-dev lib32z-dev libgl1-mesa-dev g++-multilib mingw32 tofrodos python-markdown libxml2-utils</span>

这好像是64bit版本的

我的是32位版本的，所以我装了以下包（网上找的，不过成功编译出image文件并运行了）：

<span style="color: #f18200;">sudo apt-get install git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev libc6-dev libncurses5-dev x11proto-core-dev libx11-dev libreadline5-dev libz-dev libgl1-mesa-dev</span>

<span style="color: #ff0000;">然后可以开始Building the System了（为了方便后面$PATH的设定，用sudo -s获取root权限来执行以下命令）：</span>

<span style="color: #f18200;">source build/envsetup.sh</span> <span style="color: #78b900;">// 设置环境变量</span>

<span style="color: #f18200;">lunch full-eng</span> <span style="color: #78b900;">// lunch用来选择build的目标，full-eng表示build一个用于emulator的包含debug工具的镜像，其他类型见官网</span>

<span style="color: #f18200;">make -j4</span> <span style="color: #78b900;">// 真正的make来了，-j4表示用4个线程来编，应为我的电脑是4个核的</span>

完成的时候会看到这么一行

<span style="color: #4d90fe;">Install system fs image: out/target/product/generic/system.img</span>

表示成功了

<span style="color: #f18200;">emulator</span> <span style="color: #78b900;">// 这个时候直接运行emulator，会自动加载你刚编出来的img并开机启动，然后就可以用啦</span>

现在还木有手机，期待入手<span style="color: #ff0000;">Nexus S</span>的那一天！
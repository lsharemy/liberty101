title: Ubuntu 10.04 下载Android源代码
tags:
  - Android
  - Ubuntu
id: 478
categories:
  - wordpress
  - index.php
  - csit
date: 2011-11-11 13:36:56
---

官方给出了很详细的教程：[Downloading the Source](http://source.android.com/source/downloading.html)

不过自己弄完还是总结下

<!--more-->

<span style="color: #ff0000;">首先需要获取Git和Repo：</span>

sudo apt-get install git-core curl

<span style="color: #76b900;">// Git是Linus Torvalds开发的版本控制工具，curl是用来获取repo用的</span>

curl http://android.git.kernel.org/repo &gt;~/bin/repo

<span style="color: #76b900;">// bin是自己建的一个目录，这里把repo下载到这个目录，repo是python脚本（可以打开看看）</span>

<span style="color: #76b900;">// 有关Git和Repo，可以参考</span>[Version Control with Repo and Git](http://source.android.com/source/version-control.html)

chmod a+x ~/bin/repo

<span style="color: #76b900;">// 加上可执行属性</span>

<span style="color: #ff0000;">接下来是初始化repo客户端（Initializing a Repo client）：</span>

repo init -u https://android.googlesource.com/platform/manifest

repo init -u https://android.googlesource.com/platform/manifest -b android-2.3.7_r1

<span style="color: #76b900;">// 需要给定一个url指定你想要把哪个repository下载到当前工作目录，如果没有用-b选项指定一个分支branch，则会默认最新版本的repository</span>

<span style="color: #76b900;">// 完成的时候需要你输入名字和邮箱，名字会在你提交代码的时候使用，如果需要使用Gerrit的话，则要提供一个Google账户的邮箱（呆会再研究这个）</span>

<span style="color: #ff0000;">现在可以开始下载代码啦：</span>

repo sync

<span style="color: #76b900;">// 这个命令就可以搞定了，但这个过程需要个把小时（不过我好像半个小时就完成了，学校的网有些时候还是很给力的）</span>

<span style="color: #f18200;">有个疑问：git tag是干嘛用的？</span>
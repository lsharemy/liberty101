title: Ubuntu 10.04 下编译最新vlc 1.3.0
tags:
  - DASH
  - Ubuntu
  - VLC
id: 712
categories:
  - wordpress
  - index.php
  - csit
date: 2011-12-15 16:18:49
---

因为要弄研究dash，需要装个vlc，apt-get install一个还不行，因为这样得到的vlc版本是很老的，不支持dash，至少要1.2.0版本以后的吧，关于dash，可以到[这里](http://www-itec.uni-klu.ac.at/dash)了解。

我在装vlc之前，把x264和ffmpeg也装了，当然也是下载最新版本的源码编译的。

这里不打算一步步列出遇到的错误，并给出解决方<!--more-->案。因为很多错误都是类似的。

<span style="color: #f18200;">1.</span>最多出现的错误当然就是缺少包了，这种问题好解决，apt-get install一个

<span style="color: #f18200;">2.</span>其次就是包不够新的错误，而且apt-get install提示已经是最新版本了，那怎么办？那就需要最近去找对应的包的源码，自己编译安装啦。以下是我编译vlc时遇到的例子：

<span style="color: #ff0000;">Requested 'libavcodec &gt;= 52.25.0' but version of libavcodec is 52.20.1</span>

<span style="color: #ff0000;">Requested 'xcb &gt;= 1.6' but version of XCB is 1.5</span>

编译最新的x264可以会遇到这种错误，提示yasm不是最新的。解决方法还是下载最新源码编译安装咯。

<span style="color: #f18200;">3.</span>总算把依赖包都解决了，这回可以开始make了，需要挺久，不过尽然没有错误，那么就make install吧。

<span style="color: #f18200;">4.</span>make install也搞定了？那就运行vlc，不过这个时候还有错误，不要灰心，总会有解决办法的，这回的错误是：

<span style="color: #ff0000;">vlc: error while loading shared libraries: libvlc.so.5: cannot open shared object file: No such file or directory</span>

[http://www.webupd8.org/2010/01/how-to-compile-vlc-and-vlmc-from-git-in.html](http://www.webupd8.org/2010/01/how-to-compile-vlc-and-vlmc-from-git-in.html)这里有解决方法哦，是因为编译的时候把库都编译到/usr/local/lib去了，但在/usr/lib里找不到，那么创建符号链接就好了。具体参考上面这篇文章吧。

<span style="color: #f18200;">5.</span>这个也解决了，再次运行vlc，，，什么！怎么还有错误！不要抓狂，你已经接近终点了，千万不要在这个时候放弃。这回的错误是：木有图形界面。

不过输出了几行文本信息，其中一行是<span style="color: #ff0000;">main interface error: no suitable interface module</span>，复制来搜索一下

[http://blog.csdn.net/wpdzyx2003/article/details/5401080](http://blog.csdn.net/wpdzyx2003/article/details/5401080)，[http://blog.csdn.net/wpdzyx2003/article/details/5451182](http://blog.csdn.net/wpdzyx2003/article/details/5451182)这里又有解决方法噢，原来是缺少了qt，sudo apt-get install libqt4-dev libqt4-core搞定，文章中./configure后面跟了一堆选项，不过我一个选项都没有，编译是成功的，也有图形界面了。

OK，编译VLC就是这样啦。

----------------------------------------------------------------------------

今天发现官方有编译教程，在[这里](http://wiki.videolan.org/UnixCompile)，里面把需要的依赖包都给出了，在[这里](http://wiki.videolan.org/Contrib_Status)。2011.12.16

-----------------------------------------------------------------------------

重新写了个编译过程：[http://lsharemy.com/wordpress/index.php/ubuntu/ubuntu-10-04/compile-the-latest-vlc/](http://lsharemy.com/wordpress/index.php/ubuntu/ubuntu-10-04/compile-the-latest-vlc/)
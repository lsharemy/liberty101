title: 搭建DASH环境
id: 1461
date: 2012-05-24 14:43:12
---

为了毕业，开始搞研究，先把环境搭了吧

--------------------------------------------------------

参考这个来编译x264，ffmpeg，vlc：

[http://wiki.videolan.org/User:J-b](http://wiki.videolan.org/User:J-b)

1.先装git

2.获取vlc，x264，ffmpeg代码

3.编译x264，先要下载个最新的yasm来装起，然后编译x264，木有错误

4.编译ffmpeg，选项按照J-b的教程，支持AAC音频，编译也木有错误

5.编译vlc，由于教程中给出的选项很多不能识别，所以用来这里[http://lsharemy.com/wordpress/index.php/ubuntu/ubuntu-10-04/compile-the-latest-vlc/](http://lsharemy.com/wordpress/index.php/ubuntu/ubuntu-10-04/compile-the-latest-vlc/)最后提到的选项，之后的编译步骤也参考这篇文章进行。本来以为可以省略apt-get build-dep vlc这步，实践后发现不行，编译出来的vlc不能正常运行，所以还是老老实实装来这些依赖包。

-----------------------------------------------------

编译GPAC

步骤：[http://lsharemy.com/wordpress/index.php/ubuntu/ubuntu-10-04/compile-the-latest-gpac/](http://lsharemy.com/wordpress/index.php/ubuntu/ubuntu-10-04/compile-the-latest-gpac/)

------------------------------------------------------

编译DASHEncoder

步骤：[http://lsharemy.com/wordpress/index.php/ubuntu/ubuntu-10-04/compile-the-latest-dashencoder/](http://lsharemy.com/wordpress/index.php/ubuntu/ubuntu-10-04/compile-the-latest-dashencoder/)

--------------------------------------------------------

安装web服务器

sudo apt-get install apache2

--------------------------------------------------------

研究了下NetEm：[http://www.linuxfoundation.org/collaborate/workgroups/networking/netem](http://www.linuxfoundation.org/collaborate/workgroups/networking/netem)

才发现这个在2.6内核里都有的，之前好找各种限速工具，都没找到好的T_T

这里还有个how-to：[http://lartc.org/howto/](http://lartc.org/howto/)

这个教程也不错：[http://tcn.hypert.net/tcmanual.pdf](http://tcn.hypert.net/tcmanual.pdf)

原来NetEm是tc的一个扩展，专门用来模拟网络延时和丢包等，如果要限速，需要使用tc的另外一个模块，叫tbf，令牌桶，忽忽

还有htb，[http://luxik.cdi.cz/~devik/qos/htb/](http://luxik.cdi.cz/~devik/qos/htb/)

-----------------------------------------------

开始写自适应算法了

在vlc/modules/stream_filter/dash/adaptationlogic中增加了几个文件后，需要修改vlc/modules/stream_filter/Modules.am文件，把你增加的几个文件加进去

然后重新./bootstrap并./configure，再./compile就可以啦

放心，这样不会导致整个vlc全部重新编译

只有新增的文件会被编译

------------------------------------------------
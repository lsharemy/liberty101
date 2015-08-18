title: 编译最新版本FFmpeg
id: 808
date: 2011-12-22 21:08:54
---

目前最新版本为0.9

官网在这里[http://ffmpeg.org/index.html](http://ffmpeg.org/index.html)

这里准备编一个支持AAC音频编码的FFmpeg

所以先装个libfaac库：

sudo apt-get install libfaac-dev

然后./configure --enable-libfaac --enable-nonfree

木有问题就可以make啦

怎么一个错误没有就成功了

好吧，sudo make install，搞定

----------------------------------------------------

后来用了这个：./configure --enable-gpl --enable-pthreads --enable-libmp3lame --enable-libfaac --enable-nonfree

需要先装个libmp3lame-dev
title: 编译最新版本的vlc
id: 811
date: 2011-12-22 21:26:41
---

目前的最新版本为vlc-snapshot-branch-1.2.0-20111223。

官网在这[http://www.videolan.org/vlc/](http://www.videolan.org/vlc/)

关于在Linux下的编译，官方有个教程[http://wiki.videolan.org/UnixCompile](http://wiki.videolan.org/UnixCompile)

编译vlc，比较麻烦的是需要装很多很多的库，不过还好，官方也有给出这些库[http://wiki.videolan.org/Contrib_Status](http://wiki.videolan.org/Contrib_Status)

好像那么就先从这些库开始。

官方给出了Ubuntu系统的一键安装：
<pre>$ sudo apt-get -y install libvorbis-dev libogg-dev libtheora-dev speex libspeex-dev flac libflac-dev \
  x264 libx264-dev <span style="color: #ff0000;">a52-0.7.4</span> liba52-0.7.4-dev mpeg2dec libmpeg2-4-dev faad libfaad-dev faac libfaac-dev \
  lame libmp3lame-dev ffmpeg libavdevice-dev libmad0 libmad0-dev dirac libdirac-dev liboil-dev libschroedinger-dev \
  libdca-dev twolame libtwolame-dev libmpcdec-dev libvorbisidec1 libvorbisidec-dev libass-dev libass4 <span style="color: #ff0000;">libebml2</span> \
  libebml-dev <span style="color: #ff0000;">libmatroska2</span> libmatroska-dev <span style="color: #ff0000;">libdvbpsi6</span> <span style="color: #ff0000;">libdvbpsi-dev</span> <span style="color: #ff0000;">libmodplug1</span> libmodplug-dev libshout3 libshout3-dev \
  libdvdread4 libdvdnav4 libdvdnav-dev livemedia-utils liblivemedia-dev libcddb2 libcddb2-dev libcdio10 libcdio-dev \
  libcdio-utils vcdimager libvcdinfo0 libvcdinfo-dev libgpg-error0 libgpg-error-dev libgcrypt11 libgcrypt11-dev \
  gnutls-bin libgnutls26 libgnutls-dev libdap10 libdap-bin libdap-dev libxml2 libxml2-dev libpng12-0 libpng12-dev \
  <span style="color: #ff0000;">libjpeg8</span> libtiff4 libsdl1.2-dev libsdl-image1.2 libsdl-image1.2-dev libc-bin gettext libfreetype6 libfreetype6-dev \
  libfribidi-dev libfribidi0 zlib1g zlib1g-dev libtag1-dev libcaca0 libcaca-dev caca-utils libqt4-core libqt4-dev \
  libportaudio2 libportaudio-dev <span style="color: #ff0000;">libupnp-dev</span> <span style="color: #ff0000;">libupnp4 libupnp3</span> libexpat1 libexpat1-dev yasm libxcb-xv0 libxcb-xv0-dev \
  libx11-xcb1 libx11-xcb-dev</pre>
不过在10.04的版本下还是有些错误（标为红色的包），所以我做了些修改，以下是修改以后的：（不过修改后的包版本不一定是需要的版本，如果库里面的包达不到vlc的要求，那么这些包需要自己去下载源代码来编译，具体在后面讲）
<pre>sudo apt-get -y install libvorbis-dev libogg-dev libtheora-dev speex libspeex-dev flac libflac-dev \
  x264 libx264-dev liba52-0.7.4 liba52-0.7.4-dev mpeg2dec libmpeg2-4-dev faad libfaad-dev faac libfaac-dev \
  lame libmp3lame-dev ffmpeg libavdevice-dev libmad0 libmad0-dev dirac libdirac-dev liboil-dev libschroedinger-dev \
  libdca-dev twolame libtwolame-dev libmpcdec-dev libvorbisidec1 libvorbisidec-dev libass-dev libass4 libebml0 \
  libebml-dev libmatroska0 libmatroska-dev libdvbpsi5 libdvbpsi5-dev libmodplug0c2 libmodplug-dev libshout3 libshout3-dev \
  libdvdread4 libdvdnav4 libdvdnav-dev livemedia-utils liblivemedia-dev libcddb2 libcddb2-dev libcdio10 libcdio-dev \
  libcdio-utils vcdimager libvcdinfo0 libvcdinfo-dev libgpg-error0 libgpg-error-dev libgcrypt11 libgcrypt11-dev \
  gnutls-bin libgnutls26 libgnutls-dev libdap10 libdap-bin libdap-dev libxml2 libxml2-dev libpng12-0 libpng12-dev \
  libjpeg62 libtiff4 libsdl1.2-dev libsdl-image1.2 libsdl-image1.2-dev libc-bin gettext libfreetype6 libfreetype6-dev \
  libfribidi-dev libfribidi0 zlib1g zlib1g-dev libtag1-dev libcaca0 libcaca-dev caca-utils libqt4-core libqt4-dev \
  libportaudio2 libportaudio-dev libupnp4-dev libupnp4  libexpat1 libexpat1-dev yasm libxcb-xv0 libxcb-xv0-dev \
  libx11-xcb1 libx11-xcb-dev</pre>
安装好以后，可以用dpkg -l来查看下包的版本，主要看一下之前标为红色的哪些包是否符合要求，libebml的版本为0.7.7不过要求的版本为1.2.2，libmatroska的版本为0.8.1，不过要求的版本是1.3.0。libdvbpsi的版本为0.1.6，要求的版本为0.2.2。libmodplug的版本为0.8.7不过要求的版本为0.8.8.4。libjpeg的版本为62，不过要求的版本为8c。Upnp的版本也有点问题。

不过先管这些问题，先./bootstrap一下。

<span style="color: #ff0000;">./bootstrap: 1: autoreconf: not found</span>

好吧，还忘了点东西，编译教程的开头就有这么段话you will also need the GNU build system, a.k.a. the "autotools" (autoconf, automake, libtool and gettext) to setup the Makefiles。把这里提到的几个也装上。
<pre>sudo apt-get install autoconf automake libtool gettext</pre>
好吧，这回再开始./bootstrap

Successfully bootstrapped的话就可以./configure啦

<span style="color: #ff0000;">configure: error: Could not find lua. Lua is needed for some interfaces (rc, telnet, http) as well as many other custom scripts. Use --disable-lua to ignore this error.</span>

错误来啦，lua？装！sudo apt-get install liblua5.1-0-dev lua5.1

继续./configure，下一个错误：<span style="color: #ff0000;">configure: error: Your libebml is too old: you may get a more recent one from http://dl.matroska.org/downloads/libebml/. Alternatively you can use --disable-mkv to disable the matroska plugin.</span>

这个是意料中的，因为之前已经知道这个包不够新了，只是没管它，现在去下载吧，[http://www.matroska.org/downloads/linux.html](http://www.matroska.org/downloads/linux.html)，看到libmatroska也在这个网站上，也一起下载了。make &amp;&amp; sudo make install，没有错误。

继续./configure吧，<span style="color: #ff0000;">configure: error: No package 'libpostproc' found. Pass --disable-postproc to ignore this error.</span>

缺包？装！sudo apt-get install libpostproc-dev

继续，下一个错误<span style="color: #ff0000;">checking for XCB... configure: error: Package requirements (xcb &gt;= 1.6) were not met: Requested 'xcb &gt;= 1.6' but version of XCB is 1.5</span>

xcb版本不达要求，去这里[http://xcb.freedesktop.org/](http://xcb.freedesktop.org/)下载个，编译

<span style="color: #ff0000;">checking for XCBPROTO... configure: error: Package requirements (xcb-proto &gt;= 1.6) were not met: No package 'xcb-proto' found</span>

xcb-proto也是在上面哪个网站下的，也下个1.6的吧。编译安装后，xcb也可以顺利安装了，继续。

到这个地方vlc的./configure也成功了，不过如果为了保险起见，把之前版本不够新的几个包都下载个符合要求的版本装了吧。

还可以apt-get build-dep vlc一下，又装一堆包，好吧，我也讨厌装包。对于一些重要的包的功能，在官方的编译教程页面有说明，可以看一下。

这个时候再./configure以下，就可以make &amp;&amp; sudo make install啦

没有错误，哪么vlc运行程序。

<span style="color: #ff0000;">vlc: error while loading shared libraries: libvlc.so.5: cannot open shared object file: No such file or directory</span>

创建个符号链接sudo ln -s /usr/local/lib/libvlc* /usr/lib/（学会了另外一个办法，可以在/etc/ld.so.conf里加一行/usr/local/lib就可以了）

搞定，再vlc一下，就可以运行啦。

上个图：（圣诞节快到了，所以有戴圣诞帽哈）

![](http://i.minus.com/ibwabBj1NuFXPN.png "vlc")

-----------------------------------------------------------------------

这里还有个教程：[http://wiki.videolan.org/User:J-b](http://wiki.videolan.org/User:J-b)

又编译了一次。
<pre>../configure --prefix=/usr \
        --enable-x264 --with-x264-tree=../extras/x264-trunk \
        --with-ffmpeg-mp3lame --with-ffmpeg-faac \
        --disable-postproc</pre>
发现--with-ffmpeg-mp3lame --with-ffmpeg-faac这两个选项都不能识别的。

所以就这样吧：
<pre>../configure --prefix=/usr \
        --enable-x264 --with-x264-tree=../extras/x264-trunk</pre>

-----------------------

又编译一次，这次只装了会导致configure出错的那几个包，然后也没用任何选项。
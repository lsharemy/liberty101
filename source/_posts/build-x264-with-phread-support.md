title: 'windows下编译支持多线程的x264 | build x264 with phread support under windows'
tags:
  - codec
  - H.264/AVC
  - x264
id: 145
categories:
  - wordpress
  - index.php
  - csit
date: 2011-06-29 13:59:45
---

![](http://localhostr.com/file/Hg35SDA/codec.gif "Codec")

如果你跟我一样想研究x264，那么你也和我一样，要先把x264编译通过，这编文章，就是我编译x264的全过程直播:-D，希望能帮到一些人

如果你只是想下载x264来运行下，那么去这里[http://x264.nl/](http://x264.nl/)

<!--more-->

可以先看一下扫盲贴[http://bbs.chinavideo.org/viewthread.php?tid=6945&amp;highlight=VS2008](http://bbs.chinavideo.org/viewthread.php?tid=6945&amp;highlight=VS2008)，这是关于编译x264-snapshot-20090216-2245版本的

本文使用的版本为x264-snapshot-20091006-2245版本（最后一个包含vs工程的版本），以下是编译时遇到的所有错误和解决办法（按时间顺序）：

<span style="color: #ff0000;">以下错误如没有用红色字说明，则都是由于数据声明没有放在代码块的开头造成的，修改一下即可。</span>

<span style="color: #3366ff;">先build libx264工程</span>

*   1&gt;d:\x264-snapshot-20091006-2245\common\common.h(170) :
error C2143: syntax error : missing ';' before 'type'
*   1&gt;..\..\encoder\analyse.c(2950) : error C2059: syntax
error : '['
<span style="color: #ff0000;">static const uint8_t check_mv_lists[X264_MBTYPE_MAX] =</span>
<span style="color: #ff0000;">{[P_L0]=1, [B_L0_L0]=1, [B_L1_L1]=2};</span>

<span style="color: #ff0000;">改为</span>

<span style="color: #ff0000;">static const uint8_t check_mv_lists[X264_MBTYPE_MAX] =</span>
<span style="color: #ff0000;">{0,0,0,0,1,0,0,0,1,0,0,0,2,0,0,0,0,0,0};</span>

*   1&gt;d:\x264-snapshot-20091006-2245\encoder\slicetype.c(663)
: error C2143: syntax error : missing ';' before 'type' 后续7行
*   1&gt;d:\x264-snapshot-20091006-2245\encoder\slicetype.c(752)
: error C2143: syntax error : missing ';' before 'type' 后续6行
*   1&gt;..\..\encoder\ratecontrol.c(1382) : error C2143: syntax
error : missing ';' before 'type' 后续4行
*   1&gt;..\..\common\macroblock.c(742) : error C2143: syntax
error : missing ';' before 'type' 后续5行
*   1&gt;..\..\encoder\ratecontrol.c(1514) : error C2143: syntax
error : missing ';' before 'type' 后续2行
*   1&gt;..\..\encoder\me.c(455) : error C2143: syntax error : missing
';' before 'const' 后续2行
*   1&gt;..\..\encoder\encoder.c(469) : error C2143: syntax error
: missing ';' before 'type'
*   1&gt;..\..\encoder\encoder.c(1040) : error C2275: 'uint8_t' :
illegal use of this type as an expression
*   1&gt;..\..\encoder\encoder.c(1413) : error C2143: syntax
error : missing ';' before 'type' 后续2行
*   1&gt;..\..\encoder\encoder.c(1602) : error C2143: syntax
error : missing ';' before 'type'后续2行
*   1&gt;..\..\encoder\encoder.c(1823) : error C2143: syntax
error : missing ';' before 'type'
*   1&gt;..\..\encoder\encoder.c(2245) : error C2275: 'int64_t' :
illegal use of this type as an expression 后续2行
<span style="color: #3366ff;">再build x264工程</span>

*   1&gt;..\..\muxers.c(299) : error C2146: syntax error :
missing ')' before identifier 'PRIx32'
<span style="color: #ff0000;">fprintf( stderr, "Bad header magic (%"PRIx32" &lt;=&gt; %s)\n", *((uint32_t*)header), header );</span>

<span style="color: #ff0000;">改为</span>

<span style="color: #ff0000;">fprintf( stderr, "Bad header magic (%ld &lt;=&gt;%s)\n", *((uint32_t*)header), header );</span>

<span style="color: #ff0000;">参考http://blog.csdn.net/cdjogh/archive/2010/10/24/5961665.aspx</span>

*   1&gt;..\..\muxers.c(350) : error C2275: 'AVISTREAMINFOA' :
illegal use of this type as an expression 后续2行
*   1&gt;libx264.lib(encoder.obj) : error LNK2019: unresolved
external symbol _x264_lookahead_init referenced in function
_x264_encoder_open_76

1&gt;libx264.lib(encoder.obj) : error LNK2019: unresolved
external symbol _x264_lookahead_is_empty referenced in function
_x264_encoder_encode

1&gt;libx264.lib(encoder.obj) : error LNK2019: unresolved
external symbol _x264_lookahead_get_frames referenced in function
_x264_encoder_encode

1&gt;libx264.lib(encoder.obj) : error LNK2019: unresolved
external symbol _x264_lookahead_put_frame referenced in function
_x264_encoder_encode

1&gt;libx264.lib(encoder.obj) : error LNK2019: unresolved
external symbol _x264_lookahead_delete referenced in function
_x264_encoder_close

1&gt;libx264.lib(analyse.obj) : error LNK2019: unresolved
external symbol _log2f referenced in function _x264_analyse_init_costs

1&gt;bin/x264.exe : fatal error LNK1120: 6 unresolved
externals

<span style="color: #ff0000;">这是由于libx264工程没有添加lookahead.c文件，从而缺少几个函数的定义造成的，添加lookahead.c进工程，参考[http://www.chinavideo.org/viewthread.php?tid=6914](http://www.chinavideo.org/viewthread.php?tid=6914)</span>

<span style="color: #3366ff;">再次Build libx264工程，修改以下几个错误</span>

*   1&gt;..\..\encoder\lookahead.c(135) : error C2143: syntax
error : missing ';' before 'type'
*   1&gt;..\..\encoder\lookahead.c(154) : error C2275: 'x264_t' :
illegal use of this type as an expression
*   1&gt;..\..\encoder\lookahead.c(259) : error C2143: syntax error
: missing ';' before 'type'
<span style="color: #3366ff;">再次Build x264工程</span>

*   1&gt;libx264.lib(analyse.obj) : error LNK2019: unresolved
external symbol _log2f referenced in function _x264_analyse_init_costs
<span style="color: #ff0000;">这是因为没有定义log2f函数，添加#define</span>
<span style="color: #ff0000;">log2f(x) (logf(x)*1.4426950408889634f)到osdep.h中，参考http://www.chinavideo.org/viewthread.php?tid=9715</span>

<span style="color: #3366ff;">再次Rebuild两个工程，大功告成</span>

不过这个时候编译出来的x264是不支持多线程的，如果使用—threads选项，会提示x264
[warning]: not compiled with pthread support!

<span style="color: #3366ff;">不过别灰心，总是会有解决办法的（你要始终相信这一点）</span>

因为x264的线程用的是pthread，而windows是不支持pthread的，所以需要POSIX Threads (pthreads) for Win32

这是下载地址[ftp://sourceware.org/pub/pthreads-win32](ftp://sourceware.org/pub/pthreads-win32)，选择最新的pthreads-w32-2-8-0-release.exe

解压出来后，把Pre-built.2目录下的include和lib加到VS的搜索目录中

在libx264.lib项目属性 -&gt; “C/C++” -&gt; Preprocessor中加入HAVE_PTHREAD和SYS_MINGW宏，在osdep.h中，#ifdef USE_REAL_PTHREAD之后加入一行：#pragma
comment(lib, "pthreadVC2.lib")，build成功

X264类似，添加HAVE_PTHREAD和SYS_MINGW宏，便可build成功，运行时可能会报缺少DLL，去Pre-built.2\lib下面找找吧

<span style="color: #3366ff;">现在算是真的编出来了一个支持多线程的x264了，开始慢慢研究吧</span>

多线程部分参考[http://jmvc.blog.sohu.com/145356341.html](http://jmvc.blog.sohu.com/145356341.html)
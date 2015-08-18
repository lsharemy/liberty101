title: vim + vimgdb
tags:
  - vim
id: 1674
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-14 19:27:38
---

今天有调试需要，于是把vimgdb给配置好了

参考：[http://clewn.sourceforge.net/install.html#vimgdb](http://clewn.sourceforge.net/install.html#vimgdb)

[http://easwy.com/blog/archives/advanced-vim-skills-vim-gdb-vimgdb/](http://easwy.com/blog/archives/advanced-vim-skills-vim-gdb-vimgdb/)

需要下载[vim-7.2.tar.bz2](ftp://ftp.vim.org/pub/vim/unix/vim-7.2.tar.bz2)和[vimgdb72-1.14.tar.gz](http://sourceforge.net/projects/clewn/files/vimGdb/vimgdb72-1.14/vimgdb72-1.14.tar.gz/download "Click to download vimgdb72-1.14.tar.gz")

安装步骤见：[http://clewn.sourceforge.net/install.html#vimgdb](http://clewn.sourceforge.net/install.html#vimgdb)

在编译vim的时候会出现以下错误：<!--more-->

no terminal library found
checking for tgetent()... configure: error: NOT FOUND!
You need to install a terminal library; for example ncurses.
Or specify the name of the library with --with-tlib.
make: *** [auto/config.mk] Error 1

需要装个libncurses5-dev，sudo apt-get install一下就好了

在make install之后，运行vim，会出现以下错误，这个是vim7.2的一个bug，这里找到了解决方法：[http://tech.groups.yahoo.com/group/vimdev/message/54793](http://tech.groups.yahoo.com/group/vimdev/message/54793)，

*** buffer overflow detected ***: vim terminated

---------------------------------------------------------------

具体的使用还不熟练，有待研究。

---------------------------------------
gdb_mappings.vim中默认的键映射（摘自vimgdb帮助文档）：

&lt;Space&gt; launch the interactive gdb input-line window
CTRL-Z send an interrupt to GDB and the program it is running
B info breakpoints
L info locals
A info args
S step
I stepi
CTRL-N next: next source line, skipping all function calls
X nexti
F finish
R run
Q quit
C continue
W where
CTRL-U up: go up one frame
CTRL-D down: go down one frame
CTRL-B set a breakpoint on the line where the cursor is located
CTRL-E clear all breakpoints on the line where the cursor is located
CTRL-P Normal mode: print value of word under cursor
Visual mode: GDB command "createvar" selected expression, see
|gdb-variables|
CTRL-X print value of data referenced by word under cursor

CTRL-B and CTRL-E operate both on source code and on disassembled code in
assembly buffers.
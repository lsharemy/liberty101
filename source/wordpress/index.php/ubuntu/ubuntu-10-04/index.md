title: Ubuntu 10.04
id: 799
date: 2011-12-22 20:22:02
---

中文输入法：sudo apt-get install ibus-pinyin，原来还要在Language Support里Keyboard imput method system设置为ibus，这样系统启动就会把iBus启动起来了，而且system tray里也会出现输入法图标了，妈妈再也不用担心我的输入问题^_^。
----------------------
ibus升级：
<pre>sudo add-apt-repository ppa:shawn-p-huang/ppa 
sudo apt-get update 
sudo apt-get install ibus-gtk ibus-qt4 ibus-pinyin ibus-pinyin-db-open-phrase</pre>
参考[http://code.google.com/p/ibus/wiki/PinYinUserGuideCNdd](http://code.google.com/p/ibus/wiki/PinYinUserGuideCNdd)
------------------------
右键终端：sudo apt-get install nautilus-open-terminal

星际译王：sudo apt-get install stardict

谷歌浏览器Chrome：[https://www.google.com/chrome](https://www.google.com/chrome)

Chat：Empathy -&gt; Gtalk

虚拟机：VirtualBox [https://www.virtualbox.org/](https://www.virtualbox.org/)

编辑器：sudo apt-get install vim

地址导航栏修改为显示路径：运行gconf-editor，apps-&gt;nautilus-&gt;preferences，always_use_location_entry项打勾

代码编辑调试工具：sudo apt-get install codeblocks，[http://www.codeblocks.org/](http://www.codeblocks.org/)

Code::Blocks更改控制台程序的运行终端（默认为xterm，不能复制粘贴，于是改成gnome-terminal）：Setting -&gt; Environment -&gt; General Setting -&gt; Terminal to lanuch console programs: 这里把“xterm -T $TITLE -e”改成“gnome-terminal -t $TITLE -x”

sudo apt-get install subversion

sudo apt-get install git-core

sudo apt-get install g++

在Nautilus中ctrl+H可以显示隐藏文件

UltraSurf：sudo add-apt-repository ppa:ubuntu-wine/ppa、sudo apt-get install wine1.3、winetricks-&gt;mfc42、^_^

Dropbox：wget -O - "https://www.dropbox.com/download?plat=lnx.x86" | tar xzf -、.dropbox-dist/dropboxd、sudo apt-get install python-gpgme

gedit中文乱码问题：gconf-editor-&gt;app-&gt;gedit-2-&gt;preferences-&gt;encodings中的auto_detected项，添加GB18030编码，并移到最上端
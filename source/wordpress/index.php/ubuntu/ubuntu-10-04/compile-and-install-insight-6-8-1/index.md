title: 编译安装Insight 6.8.1
id: 851
date: 2011-12-23 17:43:27
---

图形化的GDB，官网在这里：[http://sourceware.org/insight/](http://sourceware.org/insight/)

make的时候遇到以下错误：

<span style="color: #ff0000;">cc1: warnings being treated as errors</span>
<span style="color: #ff0000;"> linux-nat.c: In function ‘linux_nat_info_proc_cmd’:</span>
<span style="color: #ff0000;"> linux-nat.c:2879: error: ignoring return value of ‘fgets’, declared with attribute warn_unused_result</span>
<span style="color: #ff0000;"> make[2]: *** [linux-nat.o] Error 1</span>
<span style="color: #ff0000;"> make[2]: Leaving directory `/home/lmy/Downloads/insight-6.8-1/gdb'</span>

解决办法：

打开insight-6.8-1/gdb目录下的Makefile，找到以下这行

WERROR_CFLAGS = -Werror

将后面的-Werror删掉或注释掉，如：

WERROR_CFLAGS =

或

WERROR_CFLAGS = #-Werror

重新make一下

成功，在sudo make install就搞定啦
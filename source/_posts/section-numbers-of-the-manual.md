title: Linux在线帮助（man）的“章节”
tags:
  - Linux
id: 1945
categories:
  - wordpress
  - index.php
  - csit
date: 2012-10-17 21:35:40
---

<pre>       1   Executable programs or shell commands
       2   System calls (functions provided by the kernel)
       3   Library calls (functions within program libraries)
       4   Special files (usually found in /dev)
       5   File formats and conventions eg /etc/passwd
       6   Games
       7   Miscellaneous  (including  macro  packages and conven‐
           tions), e.g. man(7), groff(7)
       8   System administration commands (usually only for root)
       9   Kernel routines [Non standard]</pre>
通过man 1 intro可以查看1中的介绍，其他章节类似

使用方法：

查命令：man 1 ls

查系统调用：man 2 open

查库函数：man 3 printf
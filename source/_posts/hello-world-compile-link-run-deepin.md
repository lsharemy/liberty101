title: 'hello, world!程序编译链接运行深入剖析'
tags:
  - CSAPP
  - Notes
id: 1920
categories:
  - wordpress
  - index.php
  - csit
date: 2012-09-30 20:58:01
---

学C语言的第一个程序就是以下这个helloworld程序:

[c]
#include

int main()
{
    printf(&quot;hello, world!\n&quot;);
}
[/c]

它通过gcc -o hello hello.c编译，然后通过./hello就可以运行了，屏幕上打印出"hello, world\n"

不过它是如何被编译和链接的，又是如何在系统中运行的？第二次看《深入理解计算机系统》，终于明白了一点，所以做个总结。

# <span style="color: #ff0000;">编译</span>

gcc -o hello hello.c这个命令其实gcc帮你干完了编译和链接两个工作。（编译其实还包括了汇编，可以把*.c-&gt;*.s理解为编译，.s-&gt;.o理解为汇编）

如果想只编译那么要用-c选项，gcc -c hello.c，这会生成一个hello.o文件，这是一个可重定位目标文件，有关可重定位目标文件，我在[这篇文章](http://lsharemy.com/wordpress/index.php/csit/elf-file-format/)里做过总结。

这个文件由一个ELF头、一个节头部表、以及很多个节组成。<!--more-->

比较重要的节有：

.text节，程序编译后的机器代码
.rodata节，只读数据，比如字符串常量、switch语句中的跳转表等就放在这个节
.data节，已初始化的全局变量
.bss节，未初始化的全局变更
.symtab节，一个符号表，存放程序中定义和引用的函数和全局变量的信息
.rel.text节和.rel.data节存放与重定位有关的信息
.debug节和.line节，只有用-g选项进行编译才会有
，存放与调试有关的信息
.strtab，一个字符串表。

先通过objdump -d -r hello.o对该文件进行反汇编：
<pre>00000000 &lt;main&gt;:
   0:	55                   	push   %ebp
   1:	89 e5                	mov    %esp,%ebp
   3:	83 e4 f0             	and    $0xfffffff0,%esp
   6:	83 ec 10             	sub    $0x10,%esp
   9:	c7 04 24 00 00 00 00 	movl   $0x0,(%esp)
			c: R_386_32	.rodata
  10:	e8 fc ff ff ff       	call   11 &lt;main+0x11&gt;
			11: R_386_PC32	puts
  15:	c9                   	leave
  16:	c3                   	ret</pre>
-r选项同时显示出了代码重需要重定位的地方。这段程序其实就是将参数（字符常指针）入栈之后，调用打印函数。不过由于现在还不确定程序将来会在哪段地址运行，所以还没法确定字符串的地址、以及打印函数的地址，而这要到链接才能确定，到后面再讨论。

# <span style="color: #ff0000;">链接</span>

使用gcc -o hello hello.o来对其进行链接。

链接包括符号解析和重定位两步。

符号解析就是将输入的可重定位目标文件的符号表中的符号与其定义联系起来。比如我们在程序中调用了printf函数，它在系统的一个动态库中定义。

完成符号解析之后，链接器就知道输入目标模块中的代码节和数据节的确切大小了。就可以开始重定位了，这个步骤中，将合并输入模块，并为每一个符号分配运行地址。

重定位分两步组成：

1.重定位节和符号定义。这一步中，连接器将所有相同类型的节合并起来，并为它们赋予存储器地址。当这一步完成时，程序中的每个指令和全局变量都有唯一的运行时存储器地址了。
2.重定位节中的符号引用。这一步，链接器修改代码节和数据节中对每个符号的引用，使得它们指向正确的地址。

通过链接，就会生成可执行目标文件hello

再看以下hello反汇编的main部分代码：
<pre>080483e4 &lt;main&gt;:
 80483e4:	55                   	push   %ebp
 80483e5:	89 e5                	mov    %esp,%ebp
 80483e7:	83 e4 f0             	and    $0xfffffff0,%esp
 80483ea:	83 ec 10             	sub    $0x10,%esp
 80483ed:	c7 04 24 c0 84 04 08 	movl   $0x80484c0,(%esp)
 80483f4:	e8 1f ff ff ff       	call   8048318 &lt;puts@plt&gt;
 80483f9:	c9                   	leave
 80483fa:	c3                   	ret
 80483fb:	90                   	nop
 80483fc:	90                   	nop
 80483fd:	90                   	nop
 80483fe:	90                   	nop
 80483ff:	90                   	nop</pre>
看到之前需要重定位的两个地方现在都有了确切的地址了。

# <span style="color: #ff0000;">运行</span>

然后就可以通过./hello运行啦

在Linux中，每个程序都有一个运行时存储器映像，如下图所示（图中“从可执行文件中加载那个大括号括错了，应该往下平移下）：

![](http://i.minus.com/ib0c7AaN9N4Tpl.JPG "Linux")用readelf -l hello可以看到只读段和读写段分别占用哪段地址：

<pre>Program Headers:
  Type           Offset   VirtAddr   PhysAddr   FileSiz MemSiz  Flg Align
LOAD           0x000000 0x08048000 0x08048000 0x004d4 0x004d4 R E 0x1000
LOAD           0x000f0c 0x08049f0c 0x08049f0c 0x00108 0x00110 RW  0x1000</pre>
用readelf -S hello可以看到所有节的的信息：
<pre>Section Headers:
  [Nr] Name              Type            Addr     Off    Size   ES Flg Lk Inf Al
  [ 0]                   NULL            00000000 000000 000000 00      0   0  0
  [ 1] .interp           PROGBITS        08048134 000134 000013 00   A  0   0  1
  [ 2] .note.ABI-tag     NOTE            08048148 000148 000020 00   A  0   0  4
  [ 3] .note.gnu.build-i NOTE            08048168 000168 000024 00   A  0   0  4
  [ 4] .hash             HASH            0804818c 00018c 000028 04   A  6   0  4
  [ 5] .gnu.hash         GNU_HASH        080481b4 0001b4 000020 04   A  6   0  4
  [ 6] .dynsym           DYNSYM          080481d4 0001d4 000050 10   A  7   1  4
  [ 7] .dynstr           STRTAB          08048224 000224 00004a 00   A  0   0  1
  [ 8] .gnu.version      VERSYM          0804826e 00026e 00000a 02   A  6   0  2
  [ 9] .gnu.version_r    VERNEED         08048278 000278 000020 00   A  7   1  4
  [10] .rel.dyn          REL             08048298 000298 000008 08   A  6   0  4
  [11] .rel.plt          REL             080482a0 0002a0 000018 08   A  6  13  4
  [12] .init             PROGBITS        080482b8 0002b8 000030 00  AX  0   0  4
  [13] .plt              PROGBITS        080482e8 0002e8 000040 04  AX  0   0  4
  [14] .text             PROGBITS        08048330 000330 00016c 00  AX  0   0 16
  [15] .fini             PROGBITS        0804849c 00049c 00001c 00  AX  0   0  4
  [16] .rodata           PROGBITS        080484b8 0004b8 000016 00   A  0   0  4
  [17] .eh_frame         PROGBITS        080484d0 0004d0 000004 00   A  0   0  4
  [18] .ctors            PROGBITS        08049f0c 000f0c 000008 00  WA  0   0  4
  [19] .dtors            PROGBITS        08049f14 000f14 000008 00  WA  0   0  4
  [20] .jcr              PROGBITS        08049f1c 000f1c 000004 00  WA  0   0  4
  [21] .dynamic          DYNAMIC         08049f20 000f20 0000d0 08  WA  7   0  4
  [22] .got              PROGBITS        08049ff0 000ff0 000004 04  WA  0   0  4
  [23] .got.plt          PROGBITS        08049ff4 000ff4 000018 04  WA  0   0  4
  [24] .data             PROGBITS        0804a00c 00100c 000008 00  WA  0   0  4
  [25] .bss              NOBITS          0804a014 001014 000008 00  WA  0   0  4
  [26] .comment          PROGBITS        00000000 001014 000048 01  MS  0   0  1
  [27] .shstrtab         STRTAB          00000000 00105c 0000ee 00      0   0  1
  [28] .symtab           SYMTAB          00000000 0015fc 000410 10     29  45  4
  [29] .strtab           STRTAB          00000000 001a0c 0001fb 00      0   0  1</pre>
在shell中敲入./hello后，shell会先调用fork()函数创建一个新的哦进程，然后调用exec()函数来加载hello程序，加载器会删除子进程现有的虚拟存储器段，并创建一组新的代码、数据、堆和栈段。其中堆和栈被初始化为零，代码段和数据段被初始化为hello中的相应内容。最后加载器跳转到_start，而它最终会调用hello中的main函数。

（注：加载器并不会把代码段和数据段直接拷贝到内存中，而是在页表中将相应条目指向文件的适当位置，在每个页被引用时，会引发缺页中断，然后虚拟存储器系统会按照需要自动的跳入相应页）

-------------------------------------------------------------------------------------------------------

以下面这段代码为例，看下C语言中各变量的存储位置：

[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;

int g_not_init1;
int g_not_init2;
int g_init1 = 1;
int g_init2 = 2;

int main()
{
    char *str = &quot;abc&quot;;
    int local1 = 1;
    int local2 = 2;
    static int static1 = 1;
    static int static2 = 2;
    int *p = (int*)malloc(sizeof(int));
    printf(&quot;main:%p\n&quot;, main);
    printf(&quot;g_not_init1:%p\n&quot;, &amp;g_not_init1);
    printf(&quot;g_not_init2:%p\n&quot;, &amp;g_not_init2);
    printf(&quot;g_init1:%p\n&quot;, &amp;g_init1);
    printf(&quot;g_init2:%p\n&quot;, &amp;g_init2);
    printf(&quot;local1:%p\n&quot;, &amp;local1);
    printf(&quot;local2:%p\n&quot;, &amp;local2);
    printf(&quot;static1:%p\n&quot;, &amp;static1);
    printf(&quot;static2:%p\n&quot;, &amp;static2);
    printf(&quot;p:%p\n&quot;, p);
    printf(&quot;str:%p\n&quot;, str);
}
[/c]

运行结果：
<pre>main:0x8048414
g_not_init1:0x804a034
g_not_init2:0x804a030
g_init1:0x804a018
g_init2:0x804a01c
local1:0xbfeb3a78
local2:0xbfeb3a74
static1:0x804a020
static2:0x804a024
p:0x8191008
str:0x80485f0</pre>
用readelf -S hello查看下节信息：
<pre>Section Headers:
  [Nr] Name              Type            Addr     Off    Size   ES Flg Lk Inf Al
  [ 0]                   NULL            00000000 000000 000000 00      0   0  0
  [ 1] .interp           PROGBITS        08048134 000134 000013 00   A  0   0  1
  [ 2] .note.ABI-tag     NOTE            08048148 000148 000020 00   A  0   0  4
  [ 3] .note.gnu.build-i NOTE            08048168 000168 000024 00   A  0   0  4
  [ 4] .hash             HASH            0804818c 00018c 00002c 04   A  6   0  4
  [ 5] .gnu.hash         GNU_HASH        080481b8 0001b8 000020 04   A  6   0  4
  [ 6] .dynsym           DYNSYM          080481d8 0001d8 000060 10   A  7   1  4
  [ 7] .dynstr           STRTAB          08048238 000238 000053 00   A  0   0  1
  [ 8] .gnu.version      VERSYM          0804828c 00028c 00000c 02   A  6   0  2
  [ 9] .gnu.version_r    VERNEED         08048298 000298 000020 00   A  7   1  4
  [10] .rel.dyn          REL             080482b8 0002b8 000008 08   A  6   0  4
  [11] .rel.plt          REL             080482c0 0002c0 000020 08   A  6  13  4
  [12] .init             PROGBITS        080482e0 0002e0 000030 00  AX  0   0  4
  [13] .plt              PROGBITS        08048310 000310 000050 04  AX  0   0  4
  [14] .text             PROGBITS        08048360 000360 00026c 00  AX  0   0 16
  [15] .fini             PROGBITS        080485cc 0005cc 00001c 00  AX  0   0  4
  [16] .rodata           PROGBITS        080485e8 0005e8 000089 00   A  0   0  4
  [17] .eh_frame         PROGBITS        08048674 000674 000004 00   A  0   0  4
  [18] .ctors            PROGBITS        08049f0c 000f0c 000008 00  WA  0   0  4
  [19] .dtors            PROGBITS        08049f14 000f14 000008 00  WA  0   0  4
  [20] .jcr              PROGBITS        08049f1c 000f1c 000004 00  WA  0   0  4
  [21] .dynamic          DYNAMIC         08049f20 000f20 0000d0 08  WA  7   0  4
  [22] .got              PROGBITS        08049ff0 000ff0 000004 04  WA  0   0  4
  [23] .got.plt          PROGBITS        08049ff4 000ff4 00001c 04  WA  0   0  4
  [24] .data             PROGBITS        0804a010 001010 000018 00  WA  0   0  4
  [25] .bss              NOBITS          0804a028 001028 000010 00  WA  0   0  4
  [26] .comment          PROGBITS        00000000 001028 000048 01  MS  0   0  1
  [27] .shstrtab         STRTAB          00000000 001070 0000ee 00      0   0  1
  [28] .symtab           SYMTAB          00000000 001610 000480 10     29  47  4
  [29] .strtab           STRTAB          00000000 001a90 000251 00      0   0  1
Key to Flags:
  W (write), A (alloc), X (execute), M (merge), S (strings)
  I (info), L (link order), G (group), x (unknown)
  O (extra OS processing required) o (OS specific), p (processor specific)</pre>
用readelf -l hello查看下段信息，注意这里还打印出了段与节的对应关系（Section to Segment mapping）：
<pre>Elf file type is EXEC (Executable file)
Entry point 0x8048360
There are 8 program headers, starting at offset 52

Program Headers:
  Type           Offset   VirtAddr   PhysAddr   FileSiz MemSiz  Flg Align
  PHDR           0x000034 0x08048034 0x08048034 0x00100 0x00100 R E 0x4
  INTERP         0x000134 0x08048134 0x08048134 0x00013 0x00013 R   0x1
      [Requesting program interpreter: /lib/ld-linux.so.2]
  LOAD           0x000000 0x08048000 0x08048000 0x00678 0x00678 R E 0x1000
  LOAD           0x000f0c 0x08049f0c 0x08049f0c 0x0011c 0x0012c RW  0x1000
  DYNAMIC        0x000f20 0x08049f20 0x08049f20 0x000d0 0x000d0 RW  0x4
  NOTE           0x000148 0x08048148 0x08048148 0x00044 0x00044 R   0x4
  GNU_STACK      0x000000 0x00000000 0x00000000 0x00000 0x00000 RW  0x4
  GNU_RELRO      0x000f0c 0x08049f0c 0x08049f0c 0x000f4 0x000f4 R   0x1

 Section to Segment mapping:
  Segment Sections...
   00
   01     .interp
   02     .interp .note.ABI-tag .note.gnu.build-id .hash .gnu.hash .dynsym .dynstr .gnu.version .gnu.version_r .rel.dyn .rel.plt .init .plt .text .fini .rodata .eh_frame
   03     .ctors .dtors .jcr .dynamic .got .got.plt .data .bss
   04     .dynamic
   05     .note.ABI-tag .note.gnu.build-id
   06
   07     .ctors .dtors .jcr .dynamic .got</pre>
结合前面程序运行结果，很容易可以发现如下事实：

main函数的地址0x8048414属于.text节，放在只读段中

常量字符串“abc”的地址（也就是str指针指向的地址）0x80485f0属于.rodata节，也放在只读段中

已初始化的两个全局变量（地址分别为0x804a018和0x804a01c）属于.data节，放在读写段中

未初始化的两个全局变量属于.bss节，放在读写段中

两个初始化的静态变量与初始化过的全局变量一样，属于.data节

可以猜到（代码中没写），未初始化的静态变量应该和未初始化的全局变量类似了

两个局部变量的地址0xbfeb3a78和0xbfeb3a74，看下上面程序运行时的存储器映像图就明白了，局部变量是放在栈中的，而且是从高地址向低地址增长，这里都得到了验证。

最后剩下p，它由malloc分配，所以应该放在运行时的堆中，可以看到它的位置在读写段的后面

好了，以后再也不怕类似的笔试题了= =。
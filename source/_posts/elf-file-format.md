title: ELF文件格式探索
tags:
  - ELF
  - Linux
id: 1913
categories:
  - wordpress
  - index.php
  - csit
date: 2012-09-15 15:51:42
---

这两天在看《深入理解计算机系统》

今天又不小心看到fyzhao的[这篇文章](http://blog.csdn.net/fyzhao/article/details/5931999)，于是打算研究一下

先拿以下代码开刀：

[c]
int accum = 0;

int sum(int x, int y)
{
    int t = x + y;
    accum += t;
    return t;
}
[/c]

用gcc -O1 -c code.c来编译，生成code.o，用xxd来查看二进制信息：
<pre>0000000: <span style="color: #00ff00;">7f45 4c46 0101 0100 0000 0000 0000 0000</span>  .ELF............
0000010: <span style="color: #ff00ff;">0100</span> <span style="color: #ff9900;">0300</span><span style="color: #3366ff;"> 0100 0000</span> <span style="color: #00ccff;">0000 0000</span> <span style="color: #993366;">0000 0000</span>  ................
0000020: <span style="color: #ff99cc;">b800 0000</span> <span style="color: #cc99ff;">0000 0000</span> <span style="color: #ff0000;">3400</span> <span style="color: #ff99cc;">0000</span> <span style="color: #99ccff;">0000</span> <span style="color: #339966;">2800</span>  ........4.....(.
0000030: <span style="color: #00ffff;">0a00</span> <span style="color: #993366;">0700</span> <span style="color: #3366ff;">5589 e58b 450c 0345 0801 0500</span>  ....U...E..E....
0000040: <span style="color: #3366ff;">0000 005d c3</span>00 0000 <span style="color: #993366;">0047 4343 3a20 2855</span>  ...].....GCC: (U
0000050: <span style="color: #993366;">6275 6e74 7520 342e 342e 332d 3475 6275</span>  buntu 4.4.3-4ubu
0000060: <span style="color: #993366;">6e74 7535 2e31 2920 342e 342e 3300</span> <span style="color: #ff0000;">002e</span>  ntu5.1) 4.4.3...
0000070: <span style="color: #ff0000;">7379 6d74 6162 002e 7374 7274 6162 002e</span>  symtab..strtab..
0000080: <span style="color: #ff0000;">7368 7374 7274 6162 002e 7265 6c2e 7465</span>  shstrtab..rel.te
0000090: <span style="color: #ff0000;">7874 002e 6461 7461 002e 6273 7300 2e63</span>  xt..data..bss..c
00000a0: <span style="color: #ff0000;">6f6d 6d65 6e74 002e 6e6f 7465 2e47 4e55</span>  omment..note.GNU
00000b0: <span style="color: #ff0000;">2d73 7461 636b 00</span>00 <span style="color: #ff00ff;">0000 0000 0000 0000</span>  -stack..........
00000c0: <span style="color: #ff00ff;">0000 0000 0000 0000 0000 0000 0000 0000</span>  ................
00000d0: <span style="color: #ff00ff;">0000 0000 0000 0000 0000 0000 0000 0000</span>  ................
00000e0: <span style="color: #ff0000;">1f00 0000 0100 0000 0600 0000 0000 0000</span>  ................
00000f0: <span style="color: #ff0000;">3400 0000 1100 0000 0000 0000 0000 0000</span>  4...............
0000100: <span style="color: #ff0000;">0400 0000 0000 0000</span> <span style="color: #00ff00;">1b00 0000 0900 0000</span>  ................
0000110: <span style="color: #00ff00;">0000 0000 0000 0000 ec02 0000 0800 0000</span>  ................
0000120: <span style="color: #00ff00;">0800 0000 0100 0000 0400 0000 0800 0000</span>  ................
0000130: <span style="color: #ff9900;">2500 0000 0100 0000 0300 0000 0000 0000</span>  %...............
0000140: <span style="color: #ff9900;">4800 0000 0000 0000 0000 0000 0000 0000</span>  H...............
0000150: <span style="color: #ff9900;">0400 0000 0000 0000</span> <span style="color: #00ffff;">2b00 0000 0800 0000</span>  ........+.......
0000160: <span style="color: #00ffff;">0300 0000 0000 0000 4800 0000 0400 0000</span>  ........H.......
0000170: <span style="color: #00ffff;">0000 0000 0000 0000 0400 0000 0000 0000</span>  ................
0000180: <span style="color: #cc99ff;">3000 0000 0100 0000 3000 0000 0000 0000</span>  0.......0.......
0000190: <span style="color: #cc99ff;">4800 0000 2600 0000 0000 0000 0000 0000</span>  H...&amp;...........
00001a0: <span style="color: #cc99ff;">0100 0000 0100 0000</span> <span style="color: #ff99cc;">3900 0000 0100 0000</span>  ........9.......
00001b0: <span style="color: #ff99cc;">0000 0000 0000 0000 6e00 0000 0000 0000</span>  ........n.......
00001c0: <span style="color: #ff99cc;">0000 0000 0000 0000 0100 0000 0000 0000</span>  ................
00001d0: <span style="color: #3366ff;">1100 0000 0300 0000 0000 0000 0000 0000</span>  ................
00001e0: <span style="color: #3366ff;">6e00 0000 4900 0000 0000 0000 0000 0000</span>  n...I...........
00001f0: <span style="color: #3366ff;">0100 0000 0000 0000</span> <span style="color: #99ccff;">0100 0000 0200 0000</span>  ................
0000200: <span style="color: #99ccff;">0000 0000 0000 0000 4802 0000 9000 0000</span>  ........H.......
0000210: <span style="color: #99ccff;">0900 0000 0700 0000 0400 0000 1000 0000</span>  ................
0000220: <span style="color: #33cccc;">0900 0000 0300 0000 0000 0000 0000 0000</span>  ................
0000230: <span style="color: #33cccc;">d802 0000 1200 0000 0000 0000 0000 0000</span>  ................
0000240: <span style="color: #33cccc;">0100 0000 0000 0000</span> <span style="color: #ff6600;">0000 0000 0000 0000</span>  ................
0000250: <span style="color: #ff6600;">0000 0000 0000 0000 0100 0000 0000 0000</span>  ................
0000260: <span style="color: #ff6600;">0000 0000 0400 f1ff 0000 0000 0000 0000</span>  ................
0000270: <span style="color: #ff6600;">0000 0000 0300 0100 0000 0000 0000 0000</span>  ................
0000280: <span style="color: #ff6600;">0000 0000 0300 0300 0000 0000 0000 0000</span>  ................
0000290: <span style="color: #ff6600;">0000 0000 0300 0400 0000 0000 0000 0000</span>  ................
00002a0: <span style="color: #ff6600;">0000 0000 0300 0600 0000 0000 0000 0000</span>  ................
00002b0: <span style="color: #ff6600;">0000 0000 0300 0500 0800 0000 0000 0000</span>  ................
00002c0: <span style="color: #ff6600;">1100 0000 1200 0100 0c00 0000 0000 0000</span>  ................
00002d0: <span style="color: #ff6600;">0400 0000 1100 0400</span> <span style="color: #00ccff;">0063 6f64 652e 6300</span>  .........code.c.
00002e0: <span style="color: #00ccff;">7375 6d00 6163 6375 6d00</span> 0000 <span style="color: #00ff00;">0b00 0000</span>  sum.accum.......
00002f0: <span style="color: #00ff00;">0108 0000</span>                                ....</pre>
然后对照文档来分析，忽忽

首先是ELF头：<!--more-->

[c]
#define EI_NIDENT 16
typedef struct {
unsigned char e_ident[EI_NIDENT];
Elf32_Half e_type;
Elf32_Half e_machine;
Elf32_Word e_version;
Elf32_Addr e_entry;
Elf32_Off e_phoff;
Elf32_Off e_shoff;
Elf32_Word e_flags;
Elf32_Half e_ehsize;
Elf32_Half e_phentsize;
Elf32_Half e_phnum;
Elf32_Half e_shentsize;
Elf32_Half e_shnum;
Elf32_Half e_shstrndx;
} Elf32_Ehdr;
[/c]

其中的e_ident[]数组中各个字节所代表的含义如下:

名字 索引 用途
EI_MAG0 0 文件标志
EI_MAG1 1 文件标志
EI_MAG2 2 文件标志
EI_MAG3 3 文件标志
EI_CLASS 4 文件类别
EI_DATA 5 编码格式
EI_VERSION 6 文件版本
EI_PAD 7 补齐字节开始位置
EI_NIDENT 16 e_ident[]数组的大小

前四个字节是“魔数”，分别为0x7f、'E'、'L'、'F'。

EI_CLASS字段的含义如下，这里是32位的：

名字 值 意义
ELFCLASSNONE 0 非法目标文件
ELFCLASS32 1 32 位目标文件
ELFCLASS64 2 64 位目标文件

EI_DATA字段的含义如下，这里为小端模式：
名字 值 意义
ELFDATANONE 0 非法编码格式
ELFDATA2LSB 1 LSB 编码(小头编码)
ELFDATA2MSB 2 MSB 编码(大头编码)

EI_VERSION字段表明ELF文件的版本号，当前为1

EI_PAD为9字节的填充，用于扩展。

e_type的含义如下，这里是1，表示可重定位文件：

名字 值 意义
ET_NONE 0 未知文件类型
ET_REL 1 可重定位文件
ET_EXEC 2 可执行文件
ET_DYN 3 动态链接库文件
ET_CORE 4 Core 文件
ET_LOPROC 0xff00 特定处理器文件扩展下边界
ET_HIPROC 0xffff 特定处理器文件扩展上边界

e_machine的含义如下，这里为3，Intel：

名字 值 意义
EM_NONE 0 未知体系结构
EM_M32 1 AT&amp;T WE 32100
16
ELF 格式解析
EM_SPARC 2 SPARC
EM_386 3 Intel Architecture
EM_68K 4 Motorola 68000
EM_88K 5 Motorola 88000
EM_860 7 Intel 80860
EM_MIPS 8 MIPS RS3000 Big-Endian
EM_MIPS_RS4-BE 10 MIPS RS4000 Big-Endian
RESERVED 11 ~ 16 保留未用

e_version表明ELF文件的版本号，当前为1（与前面的EI_VERSION一样吧）

e_entry表示程序的入口地址，这个不是可执行文件，所以是0

e_phoff指明程序头表(program header table)开始处在文件中的偏移量。如果没
有程序头表,该值应设为0。

e_shoff指明节头表(section header table)开始处在文件中的偏移量。如果没有节
头表,该值应设为0。这里为0xb8，十进制为184。

e_flags此字段含有处理器特定的标志位。标志的名字符合”EF_machine_flag”的格
式。对于 Intel 架构的处理器来说,它没有定义任何标志位,所以 e_flags 应该为
0。

e_ehsize表示ELF 文件头大小，共0x34个字节，即52字节

e_phentsize，程序头表中表项的大小。由于本文件中没有程序头表，所以为0。

e_phnum，程序头表中的项数。由于本文件中没有程序头表，所以为 0。

e_shentsize，节头表中表项的大小。这里为 0x28。即40字节。

e_shnum，节头表中的表项数。这里为 0x0a，即10项。

e_shstrndx，节头字符串表索引，此处为7。

到这里一共52字节，与e_ehsize的值一致，以上就是ELF头了

-----------------------------------------------------------------------

接下去是节头表（Section Headers）

由ELF头中的信息可知节头表中有10项，每项40字节，偏移为0xb8。

上面分别用10个颜色标记出来了，从0xb8开始，到0x247。

每个节头的结构如下：

[c]
typedef struct {
Elf32_Word sh_name;
Elf32_Word sh_type;
Elf32_Word sh_flags;
Elf32_Addr sh_addr;
Elf32_Off sh_offset;
Elf32_Word sh_size;
Elf32_Word sh_link;
Elf32_Word sh_info;
Elf32_Word sh_addralign;
Elf32_Word sh_entsize;
} Elf32_Shdr;
[/c]

sh_name，本节的名字。整个名字的字符串并不存储在这里，它仅是一个索引号，指向“字符串表”节中的某个位置，那里存储了一个以'\0'结尾的字符串。

sh_type，本节的类型。下表给出了所有的节类型。

名字 值
SHT_NULL 0
SHT_PROGBITS 1
SHT_SYMTAB 2
SHT_STRTAB 3
SHT_RELA 4
SHT_HASH 5
SHT_DYNAMIC 6
SHT_NOTE 7
SHT_NOBITS 8
SHT_REL 9
SHT_SHLIB 10
SHT_DYNSYM 11
SHT_LOPROC 0x70000000
SHT_HIPROC 0x7fffffff
SHT_LOUSER 0x80000000
SHT_HIUSER 0xffffffff

具体含义这里不贴了，可以通过google关键字sh_type找到。

sh_flags，本节的一些属性，由一系列标志比特位组成，各个比特定义了节的不同属性，当某种属性被设置时，相应的标志位被设为1，反之则设为0。未定义的标志位被全部置0。
以下是这些标志位的列表及含义。

名字 值
SHF_WRITE 0x1
SHF_ALLOC 0x2
SHF_EXECINSTR 0x4
SHF_MASHPROC 0xf0000000

sh_addr，如果本节的内容需要映射到进程空间中去，此成员指定映射的起始地址；如果不需要映射,此值为0。

sh_offset指明了本节所在的位置，该值是节的第一个字节在文件中的位置，即相对于文件开头的偏移量。单位是字节。如果该节的类型为SHT_NOBITS的话，表明这一节的内容是空的，节并不占用实际的空间，这时sh_offset只代表一个逻辑上的位置概念，并不代表实际的内容。

sh_size，指明节的大小，单位是字节。如果该节的类型为SHT_NOBITS，此值仍然可能为非零，但没有实际的意义。

sh_link，此成员是一个索引值，指向节头表中本节所对应的位置。根据节的类型不同，本成员的意义也有所不同。

sh_info，此成员含有此节的附加信息，根据节的类型不同，本成员的意义也有所不同。

sh_addralign，此成员指明本节内容如何对齐字节，即该节的地址应该向多少个字节对齐。

sh_entsize，有一些节的内容是一张表，其中每一个表项的大小是固定的，比如符号表。对于这种表来说，本成员指定其每一个表项的大小。如果此值为0则表明本节内容不是这种表格。

--------------------------------------------------------

通过readelf命令查看以下code.o的节头表：
<pre>Section Headers:
  [Nr] Name              Type            Addr     Off    Size   ES Flg Lk Inf Al
  [ 0]                   NULL            00000000 000000 000000 00      0   0  0
  [ 1] .text             PROGBITS        00000000 000034 000011 00  AX  0   0  4
  [ 2] .rel.text         REL             00000000 0002ec 000008 08      8   1  4
  [ 3] .data             PROGBITS        00000000 000048 000000 00  WA  0   0  4
  [ 4] .bss              NOBITS          00000000 000048 000004 00  WA  0   0  4
  [ 5] .comment          PROGBITS        00000000 000048 000026 01  MS  0   0  1
  [ 6] .note.GNU-stack   PROGBITS        00000000 00006e 000000 00      0   0  1
  [ 7] .shstrtab         STRTAB          00000000 00006e 000049 00      0   0  1
  [ 8] .symtab           SYMTAB          00000000 000248 000090 10      9   7  4
  [ 9] .strtab           STRTAB          00000000 0002d8 000012 00      0   0  1
Key to Flags:
  W (write), A (alloc), X (execute), M (merge), S (strings)
  I (info), L (link order), G (group), x (unknown)
  O (extra OS processing required) o (OS specific), p (processor specific)</pre>
先看下节名字符串表项(.shstrtab)：
<pre>1100 0000 0300 0000 0000 0000 0000 0000
6e00 0000 4900 0000 0000 0000 0000 0000
0100 0000 0000 0000</pre>
sh_name为0x11

sh_type，值为0x03，即SHT_STRTAB，表明此表项指向一个字符串表。对于字符串表，我们只关心此表的起始地址和长度。

sh_offset,本节在文件中的偏移量。值是0x6e，这里是字符串表的起始位
置。

sh_size，本节的大小，即字符串表的长度，值为0x49。

根据这两个值，可以找到节名字符串节的位置在0x6e - 0xb7，可以看到里面存的都是节名字符串。而且偏移为0x11的字符串正是.shstrtab。

------------------------------------------------------------

同理，可以找到代码节(.text)的起始地址和长度分别为0x34和0x11。

段的具体内容为5589 e58b 450c 0345 0801 0500 0000 005d c3

可以通过objdump对code.o的代码段进行反编译：
<pre>00000000 &lt;sum&gt;:
   0:	55                   	push   %ebp
   1:	89 e5                	mov    %esp,%ebp
   3:	8b 45 0c             	mov    0xc(%ebp),%eax
   6:	03 45 08             	add    0x8(%ebp),%eax
   9:	01 05 00 00 00 00    	add    %eax,0x0
   f:	5d                   	pop    %ebp
  10:	c3                   	ret</pre>
可以发现，代码段的内容就是程序编译后的机器代码。

--------------------------------------------------

再看以下符号表(.symtab)

sh_type 值为2，即SHT_SYMTAB，表明此表项指向一个符号表。

sh_offset，即符号表在文件中的偏移量，值为0x0248。

sh_size，符号表的大小，值为0x90。

sh_entsize，值为0x010。即符号表中每一个表项的大小为0x010。

目标文件中的“符号表(symbol table)”所包含的信息用于定位和重定位程序中的符号定义和引用。

这里直接用readelf命令来查看下符号表：
<pre>Symbol table '.symtab' contains 9 entries:
   Num:    Value  Size Type    Bind   Vis      Ndx Name
     0: 00000000     0 NOTYPE  LOCAL  DEFAULT  UND
     1: 00000000     0 FILE    LOCAL  DEFAULT  ABS code.c
     2: 00000000     0 SECTION LOCAL  DEFAULT    1
     3: 00000000     0 SECTION LOCAL  DEFAULT    3
     4: 00000000     0 SECTION LOCAL  DEFAULT    4
     5: 00000000     0 SECTION LOCAL  DEFAULT    6
     6: 00000000     0 SECTION LOCAL  DEFAULT    5
     7: 00000000    17 FUNC    GLOBAL DEFAULT    1 sum
     8: 00000000     4 OBJECT  GLOBAL DEFAULT    4 accum</pre>
其中第7项和第8项是程序中定义的全局变量和函数

Ndx列表示此符号是和哪个节相关的，或者说定义在哪个节中，sum在代码节中，序号为1，accum在bss中，序号为4（程序中已经将accum初始化为0了，为什么还在bss中呢？后来改成accum=1试了以下，序号变成了3，说明就放到了data节中。也就是说初始化为0，系统就当成是没有初始化的？）

Value列给出了大小，sum函数的大小为17字节，accum变量为4字节

-------------------------------------------------------------------

还有几个不为空的节，也看以下吧

先是.rel.text，这个节定义了需要重定位的函数或全局变量，用readelf查看一下：
<pre>Relocation section '.rel.text' at offset 0x2ec contains 1 entries:
 Offset     Info    Type            Sym.Value  Sym. Name
0000000b  00000801 R_386_32          00000000   accum</pre>
看到只有一个条目，表示在代码段偏移为b的一个变量accum需要重定位，目前它的地址被设置成了0x00000000（也可以在上面反汇编代码中看到）。因为链接的时候才能确定accum的具体地址，到时需要修改这个地址为accum的真实地址。

-----------------------------------------------------------------

comment节应该就是个注释，不管了

-------------------------------------------------------------------

还有最后一个.strtab节，是一个字符串表，里面存了.symtab中的符号。这个例子中就是sum和accum

---------------------------------------------------------------------

好吧，终于分析完了，这只是分析了一种类型的目标文件（可重定位文件），其他还包括共享目标文件和可执行文件。

不过借助readelf工具，应该就很容易分析啦。
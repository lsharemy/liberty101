title: 变量存储位置
tags:
  - C
  - ELF
id: 1974
categories:
  - wordpress
  - index.php
  - csit
date: 2012-11-05 15:18:26
---

有关变量存储的位置，做个笔记。<!--more-->

test1.c:

[c]
extern int e1; // UND
extern int e2; // UND
int gu1; // COM
int gu2; // COM
int g1 = 1; // .data
int g2 = 2; // .data
int ef1(); // UND
int ef2(); // UND
static int sgu1; // .bbs
static int sgu2; // .bbs
static int sg1 = 1; // .data
static int sg2 = 2; // .data

char str1[] = &quot;hello, world!&quot;; // .data (size:14 including '&#92;&#48;')
char *str2 = &quot;hello, world!&quot;; // str2: .data | &quot;hello, world!&quot;: .rodata

void f1() // .text
{
    static int slu1; // .bbs
    static int slu2; // .bbs
    static int sl1 = 1; // .data
    static int sl2 = 2; // .data
    g1 = ef1();
    e1 = ef1();
}

void f2() // .text
{
    static int slu1; // .bbs
    static int slu2; // .bbs
    static int sl1 = 1; // .data
    static int sl2 = 2; // .data
    g1 = ef2();
    e1 = ef2();
    g2 = ef1();
    e2 = ef1();
}

int main() // .text
{
    char str3[] = &quot;hello, world!&quot;; // stack
    char *str4 = &quot;hello, world!&quot;; // str4: stack | &quot;hello, world!&quot;: .rodata
}
[/c]

test2.c:

[c]
int e1 = 3; // .data
int e2 = 4; // .data

int ef1() // .text
{
    return 1;
}

int ef2() // .text
{
    return 2;
}
[/c]

其中test1.o的符号表如下：
<pre>Symbol table '.symtab' contains 34 entries:
   Num:    Value  Size Type    Bind   Vis      Ndx Name
     0: 00000000     0 NOTYPE  LOCAL  DEFAULT  UND
     1: 00000000     0 FILE    LOCAL  DEFAULT  ABS test.c
     2: 00000000     0 SECTION LOCAL  DEFAULT    1
     3: 00000000     0 SECTION LOCAL  DEFAULT    3
     4: 00000000     0 SECTION LOCAL  DEFAULT    5
     5: 00000000     4 OBJECT  LOCAL  DEFAULT    5 sgu1
     6: 00000004     4 OBJECT  LOCAL  DEFAULT    5 sgu2
     7: 00000008     4 OBJECT  LOCAL  DEFAULT    3 sg1
     8: 0000000c     4 OBJECT  LOCAL  DEFAULT    3 sg2
     9: 00000000     0 SECTION LOCAL  DEFAULT    6
    10: 00000024     4 OBJECT  LOCAL  DEFAULT    3 sl2.1272
    11: 00000028     4 OBJECT  LOCAL  DEFAULT    3 sl1.1271
    12: 00000008     4 OBJECT  LOCAL  DEFAULT    5 slu2.1270
    13: 0000000c     4 OBJECT  LOCAL  DEFAULT    5 slu1.1269
    14: 0000002c     4 OBJECT  LOCAL  DEFAULT    3 sl2.1264
    15: 00000030     4 OBJECT  LOCAL  DEFAULT    3 sl1.1263
    16: 00000010     4 OBJECT  LOCAL  DEFAULT    5 slu2.1262
    17: 00000014     4 OBJECT  LOCAL  DEFAULT    5 slu1.1261
    18: 00000000     0 SECTION LOCAL  DEFAULT    8
    19: 00000000     0 SECTION LOCAL  DEFAULT    7
    20: 00000004     4 OBJECT  GLOBAL DEFAULT  COM gu1
    21: 00000004     4 OBJECT  GLOBAL DEFAULT  COM gu2
    22: 00000000     4 OBJECT  GLOBAL DEFAULT    3 g1
    23: 00000004     4 OBJECT  GLOBAL DEFAULT    3 g2
    24: 00000010    14 OBJECT  GLOBAL DEFAULT    3 str1
    25: 00000020     4 OBJECT  GLOBAL DEFAULT    3 str2
    26: 00000000    28 FUNC    GLOBAL DEFAULT    1 f1
    27: 00000000     0 NOTYPE  GLOBAL DEFAULT  UND ef1
    28: 00000000     0 NOTYPE  GLOBAL DEFAULT  UND e1
    29: 0000001c    48 FUNC    GLOBAL DEFAULT    1 f2
    30: 00000000     0 NOTYPE  GLOBAL DEFAULT  UND ef2
    31: 00000000     0 NOTYPE  GLOBAL DEFAULT  UND e2
    32: 0000004c    80 FUNC    GLOBAL DEFAULT    1 main
    33: 00000000     0 NOTYPE  GLOBAL DEFAULT  UND __stack_chk_fail</pre>
注意到未初始化的非静态全局变量gu1和gu2的Ndx字段为COM，关于这个，这里有讨论：[http://stackoverflow.com/questions/4137522/what-does-com-means-in-the-ndx-column-of-the-symtab-section](http://stackoverflow.com/questions/4137522/what-does-com-means-in-the-ndx-column-of-the-symtab-section)

UND表示外部变量或函数，需要在链接时确定它们的位置。
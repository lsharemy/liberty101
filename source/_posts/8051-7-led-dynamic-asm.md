title: 8051 共阴数码管 动态显示程序 asm
tags:
  - '8051'
id: 656
categories:
  - wordpress
  - index.php
  - csit
date: 2011-12-09 22:32:38
---

![](http://i.minus.com/i40Fp7tGHGEPC.gif "8051")

数码管其实也就是8个（7段+1点）简单的发光二极管组成的。在<span class="Apple-style-span" style="color: #000000; font-weight: bold;">[8051 发光二极管 流水灯程序 asm](http://lsharemy.com/wordpress/index.php/csit/8051-led-asm/ "Permalink to 8051 发光二极管 流水灯程序 asm")</span>中，已经知道怎么点亮发光二极管了，点亮数码管也是类似的。共阴极数码管的发光二极管的阴极为公共端，接GND，当某个发光二极管的阳极为高点平的时候，发光二极管导通，该段发光。反之，不发光。

<!--more-->

当有多个数码管的时候，因为端口有限，不能为没个发光二极管分配一个端口。所以产生的动态显示方式。即循环点亮每一个数码管，由于人眼的视觉暂留效应，认为各个数码管是稳定显示的。

下面是让六个数码管分别显示数字0、1、2、3、4、5的程序：

[assembly]
;动态扫描数码显示程序
;P0口接数据端口
;P2.2接段码锁存
;P2.3接位码锁存

LATCH1 BIT P2.2
LATCH2 BIT P2.3

ORG 00H

        MOV  20H, #3FH   ; 0
        MOV  21H, #06H   ; 1
        MOV  22H, #5BH   ; 2
        MOV  23H, #4FH   ; 3
        MOV  24H, #66H   ; 4
        MOV  25H, #6DH   ; 5

START:  CALL SCAN
        JMP  START

SCAN:   MOV  A, #0FEH    ; 扫描子程序
        MOV  R0, #20H
        SETB C
        MOV  R2, #06H

LOOP:   RLC  A

        MOV  P0, A
        SETB LATCH2
        CLR  LATCH2
        MOV  P0, @R0
        SETB LATCH1
        CLR  LATCH1
        INC  R0
        CALL DELAY
        DJNZ R2, LOOP
        MOV  R2, #07H
        RET
DELAY:  MOV  R3, #1       ; 扫描延时
D1:     MOV  R4, #2
D2:     MOV  R5, #248
        DJNZ R5, $
        DJNZ R4, D2
        DJNZ R3, D1
        RET
END
[/assembly]
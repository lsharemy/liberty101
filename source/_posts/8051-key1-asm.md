title: 8051 独立键盘程序 asm
tags:
  - '8051'
id: 685
categories:
  - wordpress
  - index.php
  - csit
date: 2011-12-11 21:42:31
---

独立按键的原理很简单，通过检测端口的电平即可判断键是否被按下。

下面这个程序检测4个独立按键的状态，并在数码管上显示哪个键被按下（1，2，3，4）

（实际应用中，通常按键的检测还需要考虑消抖，在这个程序中并没有考虑这一点）

[x86]
;独立键盘的输入程序
;P0口接数码管数据端口
;P2.2接段码锁存
;P2.3接位码锁存
;P3口接独立按键

LATCH1 BIT P2.2
LATCH2 BIT P2.3

     org  00h
     mov  p2, #80h
main:
     jnb  p3.0, s1ok  ;检测按键是否按下
     jnb  p3.1, s2ok
     jnb  p3.2, s3ok
     jnb  p3.3, s4ok

     JMP  MAIN
s1ok:mov  p0, #06H    ;写段码和位码
     SETB LATCH1
     CLR  LATCH1
     mov  p0, #11111110B
     SETB LATCH2
     CLR  LATCH2
     jmp  main
s2ok:mov  p0, #5BH
     SETB LATCH1
     CLR  LATCH1
     mov  p0,#11111110B
     SETB LATCH2
     CLR  LATCH2
     jmp  main
s3ok:mov  p0,#04FH
     SETB LATCH1
     CLR  LATCH1
     mov  p0,#11111110B
     SETB LATCH2
     CLR  LATCH2
     jmp  main
s4ok:mov  p0,#66H
     SETB LATCH1
     CLR  LATCH1
     mov  p0,#11111110B
     SETB LATCH2
     CLR  LATCH2
     jmp main
end
[/x86]
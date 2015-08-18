title: 8051 定时器程序 asm
tags:
  - '8051'
id: 665
categories:
  - wordpress
  - index.php
  - csit
date: 2011-12-09 23:56:40
---

来个1秒的定时器（4ms的定时器*250），再试一下之前没用过的共阳数码管，直接上代码：

[x86]
;定时器程序
;从9开始倒数，每秒倒数一个数字

ORG 00H
JMP START
ORG 0BH
JMP TIM0

START:  MOV  R3, #0
        MOV  R4, #9                 ; 从9开始显示
        MOV  DPTR, #TABLE
        MOV  TMOD,#01H              ; 定时器工作方式
        MOV  TH0, #HIGH(65536-4000)
        MOV  TL0, #LOW(65536-4000)  ; 初值4MS
        SETB TR0
        MOV  IE, #82H               ; 开中断
        MOV  P1, #90H               ; 先显示个9
        JMP  $

TIM0:   MOV  TH0,#HIGH(65536-4000)
        MOV  TL0,#LOW(65536-4000)
        INC  R3
        CJNE R3, #250, X1           ; 1s (4*250=1000ms)
        MOV  R3, #0
        DEC  R4
        CJNE R4, #0FFH, DIS         ; 到FFH后重新置9
        MOV  R4, #9                 ; 重9开始倒数
DIS:    MOV  A, R4
        MOVC A, @A+DPTR
        MOV  P1, A
X1:     RETI
;TABLE:  DB 3FH,06H,5BH,4FH,66H,6DH,7DH,07H,7FH,6FH    ;共阴字码表
TABLE:  DB 0C0H,0F9H,0A4H,0B0H,99H,92H,82H,0F8H,80H,90H    ;共阳字码表
END
[/x86]
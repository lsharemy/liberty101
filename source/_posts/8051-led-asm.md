title: 8051 发光二极管 流水灯程序 asm
tags:
  - '8051'
id: 650
categories:
  - wordpress
  - index.php
  - csit
date: 2011-12-09 20:20:21
---

开始玩8051，两个流水灯程序。用了查找表、软件延时等技术。

[assembly]
;流水灯程序
;使用杜邦线连接P1与LED端口

ORG 00H

START:  MOV A, #0FFH    ; 赋初值
        CLR C           ; 清零C

        MOV R2, #8      ; 循环8次，依次点亮8个灯
LOOP1:  RRC A           ; 带进位右移
        MOV P1, A       ; 累加器A送到P1口，低电平LED灯亮
        CALL DELAY      ; 延时
        DJNZ R2, LOOP1  ; 循环

        JMP START       ; 重复以上步骤

DELAY:  MOV R3, #20     ; 延时0.2秒
D1:     MOV R4, #20
D2:     MOV R5, #248
        DJNZ R5, $
        DJNZ R4, D2
        DJNZ R3, D1
        RET
END
[/assembly]

<!--more-->

[assembly]
; 查表法流水灯
; 使用杜邦线连接P1与LED端口

ORG   00H

START:  MOV   DPTR, #TABLE  ; 将表的地址存入数据指针
LOOP:   CLR   A             ; 从0开始
        MOVC  A, @A+DPTR    ; 到数据指针所指的地址取码
        CJNE  A, #01,LOOP1  ; 取出的码是否01H？否则跳到LOOP1
        JMP   START         ; 重复

LOOP1:  MOV   P1, A         ; 取出的值输出到P1端口
        CALL  DELAY         ; 延时
        INC   DPTR          ; 数据指针增1，指向下一个表项
        JMP   LOOP          ; 循环

DELAY:  MOV   R3, #20       ; 延时0.2s
D2:     MOV   R4, #20
D1:     MOV   R5, #248
        DJNZ  R5, $
        DJNZ  R4, D1
        DJNZ  R3, D2
        RET

TABLE:  DB    0FEH,0FDH,0FBH,0F7H  ;左移
        DB    0EFH,0DFH,0BFH, 7FH
        DB     7FH,0BFH,0DFH,0EFH  ;右移
        DB    0F7H,0FBH,0FDH,0FEH
        DB     00H,0FFH, 00H,0FFH  ;闪烁
        DB     01H                 ;结束码
END
[/assembly]
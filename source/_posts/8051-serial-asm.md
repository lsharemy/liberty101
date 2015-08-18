title: 8051 串行通信端口操作程序 asm
tags:
  - '8051'
id: 672
categories:
  - wordpress
  - index.php
  - csit
date: 2011-12-11 00:52:17
---

今天研究串口，收获很大，弄懂串口的原理了，刚开始结果一直不对，原来是因为12M的晶振频率弄不出9600的波特率，会有7%的误差，这也是串口异步通信中不能容忍的（最多5%），后来改成2400的波特率，就可以了。下面是代码：

对了，今天还把XP下的超级终端拷到win7上来用了，工作的很好，嘿嘿，下面这个程序运行的效果就是在超级终端上的回显（敲什么就显示什么）。还会把你敲的字符的ASCII码显示到LED灯上。亮表示1，灭表示0。

[x86]
;打开任意串口调试软件（windows下可以用超级终端）
;打开对应的串口，可以在设备管理器中看到
;设置波特率为2400，8个数据位，1个停止位，无奇偶校验
;相当于一个回显的功能，发送的任何数据都会被发回来
        ORG  0000H
        AJMP MAIN
        ORG  0023H
        AJMP RECEIVE           ; 跳转到接收中断入口
        ORG  0030H

MAIN:   MOV  TMOD, #20H        ; T1工作方式2
        MOV  TH1, #0F3H        ; 波特率2400
        MOV  SCON, #50H        ; 串口工作方式1，允许中断接受
        MOV  A, PCON
        CLR  ACC.7             ; 改变PCON的第7位(SMOD)为0，从而得到2400波特率
        MOV  PCON, A
        SETB EA                ; 打开总中断
        SETB ES                ; 打开串口中断
        SETB TR1               ; 打开定时器1
        AJMP $

RECEIVE:
        CLR  RI
        MOV  A, SBUF           ; 串口接收数据
        MOV  R0, A
        MOV  SBUF, A           ; 将接收的数据再传送给计算机
        JNB  TI,$              ; TI为1表示前一个数据已经发送完成了
        CLR  TI                ; 将TI置0表示可以继续接收下一个需要发送的字符
        MOV  A,R0

        ;送LED显示
        XRL  A, #0FFH          ; 由于发光二极管是低电平发光，所以取反
        MOV  P1, A             ; 这样发送字符的ASCII码就可以在LED上看出来了
        RETI
END
[/x86]
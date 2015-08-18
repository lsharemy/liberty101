title: 8051 步进电机 c
tags:
  - '8051'
id: 743
categories:
  - wordpress
  - index.php
  - csit
date: 2011-12-17 22:36:45
---

步进电机（不是进步电机噢- -，好吧，我的确一直把它读成进步电机了）是机电控制中一种常用的执行机构，它的用途是将电脉冲转化为角位移。通过控制脉冲个数即可控制角位移量，从而达到准确定位的目的；同时通过控制脉冲频率来控制电机转动的速度和加速度，从而达到调速的目的。

好吧，直接上一个简单转动的代<!--more-->码（这使用的1相励磁驱动，其他还有2相励磁和1-2相励磁，原理可以看[这里](http://en.wikipedia.org/wiki/Stepper_motor)）：

[c]
#include &lt;reg52.h&gt;

sbit A1=P1^0; //定义步进电机连接端口
sbit B1=P1^1;
sbit C1=P1^2;
sbit D1=P1^3;

#define Coil_A1 {A1=1;B1=0;C1=0;D1=0;}//A相通电，其他相断电
#define Coil_B1 {A1=0;B1=1;C1=0;D1=0;}//B相通电，其他相断电
#define Coil_C1 {A1=0;B1=0;C1=1;D1=0;}//C相通电，其他相断电
#define Coil_D1 {A1=0;B1=0;C1=0;D1=1;}//D相通电，其他相断电
#define Coil_OFF {A1=0;B1=0;C1=0;D1=0;}//全部断电

unsigned char Speed;
/*------------------------------------------------
 uS延时函数，含有输入参数 unsigned char t，无返回值
 unsigned char 是定义无符号字符变量，其值的范围是
 0~255 这里使用晶振12M，精确延时请使用汇编,大致延时
 长度如下 T=tx2+5 uS
------------------------------------------------*/
void DelayUs2x(unsigned char t)
{
    while(--t);
}
/*------------------------------------------------
 mS延时函数，含有输入参数 unsigned char t，无返回值
 unsigned char 是定义无符号字符变量，其值的范围是
 0~255 这里使用晶振12M，精确延时请使用汇编
------------------------------------------------*/
void DelayMs(unsigned char t)
{
    while(t--)
    {
        //大致延时1mS
        DelayUs2x(245);
        DelayUs2x(245);
    }
}
/*------------------------------------------------
                    主函数
------------------------------------------------*/
main()
{
    //unsigned int i=64*16; //转2周停止
    Speed=5; //调整速度
    while(1)
    {
        Coil_A1                 //遇到Coil_A1  用{A1=1;B1=0;C1=0;D1=0;}代替
        DelayMs(Speed);         //改变这个参数可以调整电机转速 ,
                                //数字越小，转速越大,力矩越小
        Coil_B1
        DelayMs(Speed);
        Coil_C1
        DelayMs(Speed);
        Coil_D1
        DelayMs(Speed);
    }
}
[/c]
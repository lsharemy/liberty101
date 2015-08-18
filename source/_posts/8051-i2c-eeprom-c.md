title: 8051 I2C EEPROM c
tags:
  - '8051'
id: 887
categories:
  - wordpress
  - index.php
  - csit
date: 2011-12-25 16:41:30
---

终于又接触到I2C总线了，记得研一的时候去nVidia面试的时候就被问到有没写过I2C的驱动程序，当时只能说木有。后来在上嵌入式课的时候也有讲到这个，只是这个课后来几乎都跷掉了。没想到学8051的时候又碰到了。而且这次<!--more-->有程序啦。下次面试再被问到就不怕啦，忽忽。

EEPROM因为掉电不消失，所以用来存一些需要掉电保存的信息。下面这个程序就是8051通过I2C总线与EEPROM交互的过程：

[c]
/*-----------------------------------------------
  IIC协议 EEPROM24c02 存数读取数据
  这里用8个LED显示数据
  函数是采用软件延时的方法产生SCL脉冲,固对高晶振频率要作一定的修改
  (本例是1us机器周期,即晶振频率要小于12MHZ)
------------------------------------------------*/

#include &lt;reg52.h&gt;          //头文件的包含
#include &lt;intrins.h&gt;

#define  _Nop()  _nop_()        //定义空指令

// 常,变量定义区

sbit SDA=P2^1;            //模拟I2C数据传送位
sbit SCL=P2^0;            //模拟I2C时钟控制位

bit ack;	              //应答标志位

void DelayUs2x(unsigned char t);//函数声明
void DelayMs(unsigned char t);

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
                    启动总线
------------------------------------------------*/
void Start_I2c()
{
     SDA=1;   //发送起始条件的数据信号
     _Nop();
     SCL=1;
     _Nop();    //起始条件建立时间大于4.7us,延时
     _Nop();
     _Nop();
     _Nop();
     _Nop();
     SDA=0;     //发送起始信号
     _Nop();    //起始条件锁定时间大于4μ
     _Nop();
     _Nop();
     _Nop();
     _Nop();
     SCL=0;    //钳住I2C总线，准备发送或接收数据
     _Nop();
     _Nop();
}
/*------------------------------------------------
                    结束总线
------------------------------------------------*/
void Stop_I2c()
{
    SDA=0;    //发送结束条件的数据信号
    _Nop();   //发送结束条件的时钟信号
    SCL=1;    //结束条件建立时间大于4μ
    _Nop();
    _Nop();
    _Nop();
    _Nop();
    _Nop();
    SDA=1;    //发送I2C总线结束信号
    _Nop();
    _Nop();
    _Nop();
    _Nop();
}
/*----------------------------------------------------------------
                 字节数据传送函数
函数原型: void  SendByte(unsigned char c);
功能:  将数据c发送出去,可以是地址,也可以是数据,发完后等待应答,并对
     此状态位进行操作.(不应答或非应答都使ack=0 假)
     发送数据正常，ack=1; ack=0表示被控器无应答或损坏。
------------------------------------------------------------------*/
void  SendByte(unsigned char c)
{
    unsigned char BitCnt;

    for(BitCnt=0;BitCnt&lt;8;BitCnt++)  //要传送的数据长度为8位
    {
        if((c&lt;&lt;BitCnt)&amp;0x80)SDA=1;   //判断发送位
        else  SDA=0;
        _Nop();
        SCL=1;               //置时钟线为高，通知被控器开始接收数据位
        _Nop();
        _Nop();             //保证时钟高电平周期大于4μ
        _Nop();
        _Nop();
        _Nop();
        SCL=0;
    }

    _Nop();
    _Nop();
    SDA=1;               //8位发送完后释放数据线，准备接收应答位
    _Nop();
    _Nop();
    SCL=1;
    _Nop();
    _Nop();
    _Nop();
    if(SDA==1) ack=0;
    else ack=1;        //判断是否接收到应答信号
    SCL=0;
    _Nop();
    _Nop();
}
/*----------------------------------------------------------------
                 字节数据传送函数
函数原型: unsigned char  RcvByte();
功能:  用来接收从器件传来的数据,并判断总线错误(不发应答信号)，
     发完后请用应答函数。
------------------------------------------------------------------*/
unsigned char  RcvByte()
{
    unsigned char retc;
    unsigned char BitCnt;

    retc=0;
    SDA=1;             //置数据线为输入方式
    for(BitCnt=0;BitCnt&lt;8;BitCnt++)
    {
        _Nop();
        SCL=0;       //置时钟线为低，准备接收数据位
        _Nop();
        _Nop();      //时钟低电平周期大于4.7us
        _Nop();
        _Nop();
        _Nop();
        SCL=1;       //置时钟线为高使数据线上数据有效
        _Nop();
        _Nop();
        retc=retc&lt;&lt;1;
        if(SDA==1)retc=retc+1; //读数据位,接收的数据位放入retc中
        _Nop();
        _Nop();
    }
    SCL=0;
    _Nop();
    _Nop();
    return(retc);
}
/*----------------------------------------------------------------
                     应答子函数
原型:  void Ack_I2c(void);
----------------------------------------------------------------*/
void Ack_I2c(void)
{
    SDA=0;
    _Nop();
    _Nop();
    _Nop();
    SCL=1;
    _Nop();
    _Nop();              //时钟低电平周期大于4μ
    _Nop();
    _Nop();
    _Nop();
    SCL=0;               //清时钟线，钳住I2C总线以便继续接收
    _Nop();
    _Nop();
}
/*----------------------------------------------------------------
                     非应答子函数
原型:  void NoAck_I2c(void);
----------------------------------------------------------------*/
void NoAck_I2c(void)
{
    SDA=1;
    _Nop();
    _Nop();
    _Nop();
    SCL=1;
    _Nop();
    _Nop();              //时钟低电平周期大于4μ
    _Nop();
    _Nop();
    _Nop();
    SCL=0;                //清时钟线，钳住I2C总线以便继续接收
    _Nop();
    _Nop();
}
/*----------------------------------------------------------------
                    向有子地址器件发送多字节数据函数
函数原型: bit  ISendStr(unsigned char sla,unsigned char suba,ucahr *s,unsigned char no);
功能:     从启动总线到发送地址，子地址,数据，结束总线的全过程,从器件
          地址sla，子地址suba，发送内容是s指向的内容，发送no个字节。
           如果返回1表示操作成功，否则操作有误。
注意：    使用前必须已结束总线。
----------------------------------------------------------------*/
bit ISendStr(unsigned char sla,unsigned char suba,unsigned char *s,unsigned char no)
{
    unsigned char i;

    Start_I2c();               //启动总线
    SendByte(sla);             //发送器件地址
    if(ack==0) return(0);
    SendByte(suba);            //发送器件子地址
    if(ack==0) return(0);

    for(i=0;i&lt;no;i++)
    {
        SendByte(*s);          //发送数据
        if(ack==0)return(0);
        s++;
    }
    Stop_I2c();                //结束总线
    return(1);
}
/*----------------------------------------------------------------
                    向有子地址器件读取多字节数据函数
函数原型: bit  ISendStr(unsigned char sla,unsigned char suba,ucahr *s,unsigned char no);
功能:     从启动总线到发送地址，子地址,读数据，结束总线的全过程,从器件
          地址sla，子地址suba，读出的内容放入s指向的存储区，读no个字节。
           如果返回1表示操作成功，否则操作有误。
注意：    使用前必须已结束总线。
----------------------------------------------------------------*/
bit IRcvStr(unsigned char sla,unsigned char suba,unsigned char *s,unsigned char no)
{
    unsigned char i;

    Start_I2c();               //启动总线
    SendByte(sla);             //发送器件地址
    if(ack==0)return(0);
    SendByte(suba);            //发送器件子地址
    if(ack==0)return(0);

    Start_I2c();
    SendByte(sla+1);
        if(ack==0)return(0);

    for(i=0;i&lt;no-1;i++)
    {
        *s=RcvByte();             //发送数据
        Ack_I2c();                //发送就答位
        s++;
    }
    *s=RcvByte();
    NoAck_I2c();                  //发送非应位
    Stop_I2c();                   //结束总线
    return(1);
}
/*------------------------------------------------
                    主函数
------------------------------------------------*/
void main()
{
    unsigned char lmy;       // 定义临时变量
    unsigned char i;

    IRcvStr(0xae,4,&amp;lmy,1);  //调用存储数据

    while(1)
    {
        P1=lmy;              //数值用二进制显示，直接用8个LED表示
        for(i=0;i&lt;5;i++)
            DelayMs(200);
        lmy++;               //1s钟变量自加1，改变值后存储到24c02
                             //下次开机时将显示当前数值
    ISendStr(0xae,4,&amp;lmy,1); //写入24c02
    }
}
[/c]
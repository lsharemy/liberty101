title: 判断一个整数是否为无符号数(unsigned)
tags:
  - C
id: 1960
categories:
  - wordpress
  - index.php
  - csit
date: 2012-10-22 10:18:00
---

#define ISUNSIGNED(a) (a &gt;=0 &amp;&amp; (a=~a,a &gt;=0 ? (a=~a,1):(a=~a,0)))

具体分析见：[http://www.cnblogs.com/xkfz007/archive/2012/03/27/2420172.html](http://www.cnblogs.com/xkfz007/archive/2012/03/27/2420172.html)
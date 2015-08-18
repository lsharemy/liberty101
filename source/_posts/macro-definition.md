title: '宏定义（#define）总结'
tags:
  - C
id: 1980
categories:
  - wordpress
  - index.php
  - csit
date: 2012-11-06 14:19:45
---

简单的宏替换：

#define forever for(;;) // 无限循环

带参数的宏定义：

#define MAX(a, b) ((A) &gt; (B) ? (A) : (B))

注意要加括号以保证计算次序的正确性

在C表标准库中，很多实用宏的例子，比如stdio.h中的getchar和putchar，还有ctype.h中的函数，islower/isupper/...等等

还有就是条件包含和条件编译
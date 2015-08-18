title: extern修饰的函数
tags:
  - C
id: 1978
categories:
  - wordpress
  - index.php
  - csit
date: 2012-11-05 15:26:03
---

函数定义默认都是extern的，所以显示的指定extern是可选的

但如果想该函数只能在本文件中使用，那么就需要加static关键字

[http://stackoverflow.com/questions/496448/how-to-correctly-use-the-extern-keyword-in-c](http://stackoverflow.com/questions/496448/how-to-correctly-use-the-extern-keyword-in-c)

这样理解，应该是对的吧？
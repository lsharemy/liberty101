title: 'LightOJ | Math - Chinese Remainder Theorem && Extended Euclid'
tags:
  - LightOJ
id: 610
categories:
  - wordpress
  - index.php
  - csit
date: 2011-11-30 23:30:54
---

![](http://i.minus.com/ibsGwYNzCCYHXr.png "LightOJ")

今天改变了做题计划，选择了只有一个题的Chinese Remainder Theorem和Extended Euclid，看来这个选择是对的，虽然这一整天也就做了这两个题，不过收获还是很大的。

<!--more-->

其实这两个题应该都属于数论的。

1319 - Monkey Tradition | Chinese Remainder Theorem

中国剩余定理，求助了YX，给我讲了思路，不过不是太懂，后来突然意识到，和扩展欧几里德有点像么，就打开了《算法导论》，还真是，而且之前想实现书上的扩展欧几里德算法的，结果没想到办法，今天YX写的代码正是那个实现，于是照着书又实现了一遍，知道了怎么用它来求数论倒数，成功一次AC。

<span style="color: #ff0000;">1306 - Solutions to an Equation | Extended Euclid</span>

之后开始做这个题，以为比上面的会简单点，结果发现我错了，30多次历史提交记录，只有2次AC，就说明了这个题不简单，还是求助了YX，有高人在，自己都不想动脑了，呵呵，不过告人只是指点下，实现还是要靠自己，几番周折，终于通过了示例，不过总觉得不对，提交一看，RTE，第一次遇到这个错误，原来是除零造成的，再改，WA，再改，WA，，，如此循环，改了7次，其中出现了各种错误，都一一在YX的帮助下找到了，再次感谢YX，嘿嘿。
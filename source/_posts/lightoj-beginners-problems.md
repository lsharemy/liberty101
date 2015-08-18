title: 'LightOJ | Beginners Problems'
tags:
  - LightOJ
id: 575
categories:
  - wordpress
  - index.php
  - csit
date: 2011-11-28 00:56:57
---

![](http://i.minus.com/ibsGwYNzCCYHXr.png "LightOJ")

花了11天时间（17日到27日）， 把LightOJ的Beginners Problems做完了，总结下

一共36道题，其中有7道Basic Geometry，7道Basic Math，3道Number Theory，2道Basic Data Structures，和1道Binary Search/Bisection（不过这一道我还没发现和二分搜索有什么关系），剩下来36-7-7-3-2-1=16道是纯粹的Beginners Problems。

<span style="color: #ff0000;">又多出来两道，切掉，一道是纯粹的Beginners Problems，另一道属于Basic Math。</span>

好久没做题了= =，把最新的新手题1433切掉。

<!--more-->

好吧，下面一道一道总结：

1000 - Greetings from LightOJ | Beginners Problems

经典的a+b问题， 不过还WA了一次，因为没有遵循输出规则

1001 - Opposite Task | Beginners Problems

a+b的反问题，引入了special judge的概念，也WA了一次，因为没有注意到a，b也有上限

1006 - Hex-a-bonacci | Beginners Problems

让你优化一段代码，让其在运算过程中不会越界，很多涉及到大数运算的题，会让你把结果mod一数，这个题就是处理这个问题，使运算过程中不越界，WA两次

1008 - Fibsieve's Fantabulous Birthday | Basic Math, Beginners Problems

没看清题意，导致WA了四次，才晓得是一道Basic Math题，需要简单推导下

1010 - Knights in Chessboard | Basic Math, Beginners Problems

Chess中的Knight问题，知道了每个题都有个Forum，于是每个题都去看看有人提问没有，能得到一些提示，于是一次AC了，这道题还是有点小trick的

1015 - Brush (I) | Beginners Problems

纯粹的Beginners Problems，一次AC

1022 - Circle in Square | Basic Geometry, Beginners Problems

第一个几何题，算面积，学习了，一次AC

1042 - Binary Weight | Beginners Problems

位操作的题，把之前TC上看到[bmerry](http://www.topcoder.com/tc?module=MemberProfile&amp;cr=251074&amp;tab=alg)写的**[A bit of fun: fun with bits](http://community.topcoder.com/tc?module=Static&amp;d1=tutorials&amp;d2=bitManipulation)**用上了，开心，一次AC

1045 - Digits of Factorial | Beginners Problems, Number Theory

第一个数论题，知道怎么算阶乘的位数了（各种进制下），一次AC

1053 - Higher Math | Beginners Problems

给三个边，算是不是直角三角形，一次AC

1069 - Lift | Beginners Problems

一次AC

1072 - Calm Down | Basic Geometry, Beginners Problems, Binary Search/Bisection

第二个几何题，一个很大的疑问，为什么把这个题归类到Binary Search/Bisection？不过还是一次AC

1107 - How Cow | Basic Geometry, Beginners Problems

判断点是不是在矩形里面，AC

1109 - False Ordering | Beginners Problems, Number Theory

按一个数的因子个数来排序，AC

1113 - Discover the Web | Basic Data Structures, Beginners Problems

第一个数据结构题，stack，因为把while写成了if，WA了一次，第一次使用了Forum求助，而且24小时内得到了答复，很开心

1116 - Ekka Dokka | Basic Math, Beginners Problems

分蛋糕，简单思考即可解决，AC

1133 - Array Simulation | Beginners Problems

对数组的一些操作，如反转什么的，AC

1136 - Division by 3 | Beginners Problems

找到规律，就好办，AC

1182 - Parity | Beginners Problems

判断一个数的二进制表示中1的个数是奇数还是偶数，AC

1189 - Sum of Factorials | Beginners Problems

这是个Beginners Problems，竟然提交了N次，先是LTE，求助了YX，给我写了个用map的方法，结果MLE，又修改成了用pair数组，终于AC。第二天，想自己实现下，结果WA了三次，哎，还须努力啊。

1202 - Bishops | Beginners Problems

chess中的Bishop问题，WA了一次，因为少考虑了一点

1211 - Intersection of Cubes | Basic Geometry, Beginners Problems

少考虑一点，WA一次

1212 - Double Ended Queue | Basic Data Structures, Beginners Problems

数据结构，双向队列，AC

1214 - Large Division | Basic Math, Beginners Problems, Number Theory

大整数除法，没考虑中间变量的越界，WA一次，终于自己写了个大整数相除了

1216 - Juice in the Glass | Basic Geometry, Beginners Problems

算体积，AC

1225 - Palindromic Numbers (II) | Beginners Problems

回文，AC

1227 - Boiled Eggs | Beginners Problems

AC

1241 - Pinocchio | Beginners Problems

AC

1249 - Chocolate Thief | Beginners Problems

AC

1261 - K-SAT Problem | Beginners Problems

竟然第一次接触SAT问题，因为本科和研究生的算法课都翘了N节，AC

1294 - Positive Negative Sign | Basic Math, Beginners Problems

AC

1305 - Area of a Parallelogram | Basic Geometry, Beginners Problems

算平行四边形的第四个点及其面积，AC，学到了一个算三角形面积的方法（wiki里有很多）

1311 - Unlucky Bird | Basic Math, Beginners Problems

一个有关速度、加速度、距离的题，AC

1331 - Agent J | Basic Geometry, Beginners Problems

WA两次，因为没考虑到钝角，用了asin，用acos就ok啦，余弦定理，看三角函数的wiki

1338 - Hidden Secret! | Beginners Problems

新学了几招，还有几个函数，AC

1354 - IP Checking | Basic Math, Beginners Problems

二进制转十进制，还用了strtok函数，AC

<span style="color: #ff0000;">1387 - Save Setu | Beginners Problems</span>

<span style="color: #ff0000;">AC</span>

<span style="color: #ff0000;">1414 - February 29 | Basic Math, Beginners Problems</span>

<span style="color: #ff0000;">AC</span>

1433 - Minimum Arc Distance | Basic Geometry, Beginners Problems

余弦定理，AC
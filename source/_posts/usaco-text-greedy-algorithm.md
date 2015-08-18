title: USACO - TEXT Greedy Algorithm
tags:
  - USACO
id: 1646
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-13 11:53:33
---

这课是讲贪心的，贪心的代码很容易敲，不过贪心的一个关键问题是如何判断算法的正确性，因为很多问题是不能用贪心来解决的。<!--more-->

证明贪心的正确性，往往采用假设的方法。假设一个正确的结果与贪心算法得出的结果有着不同的配置，然后你可以通过一个过程，从而得到一个更好的解，所以假设不成立，说明贪心算法是正确的。

还举了几个例子，比如一个只包含三个值的序列排序，求最小交换次数。还有拓扑排序。还举了一个反例，就是硬币问题，对于一个货币系统，要得到一个给定的值，贪心是不一定正确的，要看这个货币系统是怎么样的，只是对于实际使用中的货币系统，这个贪心是正确的。
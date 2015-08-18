title: 'LightOJ | Graph Theory - Cycles/Topological Sorting/Strongly Connected Component'
tags:
  - LightOJ
id: 1425
categories:
  - wordpress
  - index.php
  - csit
date: 2012-05-05 10:00:01
---

![](http://d.localhostr.com/file/somUViW/main.png "LightOJ")

又完成一个类别（其实还有一题没做，BITMASK DP，现在感觉功力还不够，先留着吧），发现这个类别还是因为JX问我1003，本来已经去做Dijkstra/Floyd Warshall了。好吧，总结下：<!--more-->

1003 - Drunk

找环的题，只要找到环就可以了，刚开始自己WA了一次，后来从YX那得知，一遍dfs就可以找出所有环

1034 - Hit the Light Switches

拓扑排序的应用，刚开始和强连通分量搞混，WA了一次

1168 - Wishing Snake

强连通分量的应用，WA一次，数组未清零，还有没有考虑只有一个强连通分量的情况要单独讨论

1210 - Efficient Traffic System

相当于求在一个有向图中加多少条边，可以使得这个图任意两点都可达（整个图成为一个强连通分量），只知道要求强连通分量，但求出来发现不好处理。后来就google了，原来在新构建的DAG中，分别求出入度和出度为0的节点个数，最后取个最大就好了。学习了。对了，还WA了一次，因为拷贝自己的代码时，忘记把adj改成adjSCC了，所以每次要复制代码的时候要注意了，好多次这样了

1390 - Weight Comparison

最伤的一题来了，这是到那天为止LightOJ上仅存的一道还没有AC的题了，于是那天我已经做好了一天做一题的打算（本来应该做好一天做0题的打算的），下午和晚上，和YX两个人，提交了N次，都木有看到一个AC，晚上到12点多，终于决定放弃，于是去论坛提问，Jan-e给了点提示，终于恍然大悟。2次MLE，1次RE，4次TLE，其中RE那次卡了很久的时间，代码里好多bug，又有复制粘贴忘改变量名或函数名的错误。

<span style="color: #ff0000;">1406 - Assassin`s Creed | Dynamic Programming</span>

<span style="color: #ff0000;">占位</span>

1417 - Forwarding Emails

又用了YX大神教我的一遍DFS找出所有环的方法，忽忽。终于写出了一次bug-free的代码，一次AC
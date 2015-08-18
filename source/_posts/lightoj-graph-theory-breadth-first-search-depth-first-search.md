title: 'LightOJ | Graph Theory - Breadth First Search/Depth First Search'
tags:
  - LightOJ
id: 1369
categories:
  - wordpress
  - index.php
  - csit
date: 2012-04-09 22:10:47
---

![](http://d.localhostr.com/file/somUViW/main.png "LightOJ")

深搜广搜终于搜完了，一共25道题，收获很大，终于图论有点入门了。只有2题同时属于另外的类别，一个是1174，一个是1257，<span style="color: #ff0000;">红色</span>标出。

1009 - Back to Underworld | Breadth First Search, Depth First Search<!--more-->

思路错误WA一次，数组越界RE一次，少个+号WA一次，多个max又WA一次，最后终于AC，用的算法导论上的广搜模板<!--more-->

1012 - Guilty Prince | Breadth First Search, Depth First Search

广搜，求可以填充的格子数，一次AC

1039 - A Toy Company | Breadth First Search, Depth First Search

需要抽象一下，把字母的一个排序当成一个节点，排序之间可以转换的话，就想成是边，被禁止的排序则是墙，然后bfs，一次AC

1046 - Rider | Breadth First Search, Depth First Search

bfs求最短路

1049 - One Way Roads | Breadth First Search, Depth First Search

第一次用了dfs，不过这个题也就只有一条路，也就相当于个递归，一次AC

1055 - Going Together | Breadth First Search, Depth First Search

额，这题代码写的好庞大，不过还算清晰，这个题一次AC的时候，还是由点成就感的

1066 - Gathering Food | Breadth First Search, Depth First Search

WA一次，因为没有处理还字母与字母之间的障碍关系，然后AC

1094 - Farthest Nodes in a Tree | Breadth First Search, Depth First Search

看了论坛的讨论，知道了这么求一棵树上最远的两个节点，这个题也用来3种方法来实现，相当有收获！！！感谢YX的帮助

1111 - Best Picnic Ever | Breadth First Search, Depth First Search

数组开小，导致RE两次，不过vector也有忘clear

1115 - Filling the Regions | Breadth First Search, Depth First Search

TLE四次，WA一次，终于AC

1141 - Number Transformation | Breadth First Search, Depth First Search

在布丁写的题，不小心提交了两次

1165 - Digit Dancing | Breadth First Search, Depth First Search

TLE了3次，最有求助了YX，有收获不少

<span style="color: #ff0000;">1174 - Commandos | Breadth First Search, Depth First Search, Dijkstra/Floyd Warshall</span>

<span style="color: #ff0000;">忘记clear又WA一次，然后d写成s，WA一次，这个题还属于Dijkstra/Floyd Warshall，到时再看一下，相当于求2个源的最短路</span>

1175 - Jane and the Frost Giants  | Breadth First Search, Depth First Search

scanf的时候没有分配\0的空间，MLE了一次，WA了一次，YX交是RE，原来这就是scanf函数用错的效果

1185 - Escape  | Breadth First Search, Depth First Search

WA一次，还去lxjx神那求了代码

1238 - Power Puff Girls   | Breadth First Search, Depth First Search

深搜，一次AC

<span style="color: #ff0000;">1257 - Farthest Nodes in a Tree (II) | Breadth First Search, Depth First Search, Dynamic Programming</span>

<span style="color: #ff0000;">更1094类似的思路，AC，不过还属于动态规划？下次继续做动归的时候再看下</span>

1263 - Equalizing Money  | Breadth First Search, Depth First Search

WA2次，又忘记clear

1271 - Better Tour | Breadth First Search, Depth First Search

WA了4次，初始化问题又犯了

1337 - The Crystal Maze | Breadth First Search, Depth First Search

分区域的广搜，一次AC

1353 - Paths in a Tree | Breadth First Search, Depth First Search

在我AC之前，只有2个人A了这道题，压力很大，在一些细节上WA了3次，最后求助论坛，通过一个critical case找到了最后的bug，对了这个是深搜

1357 - Corrupted Friendship | Breadth First Search, Depth First Search

先是两个for循环没优化，TLE，后来RE，感觉是dfs太深，stack溢出造成的，把一个vector移出去还是RE，准备用非递归重写，后来突然想到，如果把临时变量和参数降到最小，可以不用非递归，于是试了一下AC，收获很大，知道了递归层数太多stack是会溢出的，然后大概会估算需要的stack大小了

1368 - Truchet Tiling | Breadth First Search, Depth First Search

WA一次，求助YX，不过后来自己找到了BUG

1377 - Blade and Sword | Breadth First Search, Depth First Search

这个题不错，WA两次，终于AC

1412 - Visiting Islands | Breadth First Search, Depth First Search

思路是对的，但是提交还是RE了，发现又是100000个点的深搜，stack溢出，后来又WA了2次，最后一次又是clear的问题
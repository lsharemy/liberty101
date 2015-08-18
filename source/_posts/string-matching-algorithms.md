title: 字符串匹配算法总结
tags:
  - Algorithm
id: 1931
categories:
  - wordpress
  - index.php
  - csit
date: 2012-10-03 14:48:50
---

字符串匹配问题就是要在一段文本T中，找出所有模式P出现的所有位置。

假设T的长度为n，P的长度为m，m &lt;= n

# <span style="color: #ff0000;">蛮力法</span>

对于每一个位置s（0 &lt;= s &lt;= n-m），从左到右比较P[1...m]和T[s+1...s+m]是否相同，如果相同，s就是一个有效位移。最坏情况，时间复杂度为O((n-m+1)m)。

# <span style="color: #ff0000;">Rabin-Karp算法(1987)<!--more--></span>

将所有字符都赋予d进制数字（d为字符集的大小），然后模式P[1...m]就可以转换成一个d进制数p，转换的的复杂度为O(m)，令ts表示T[1...n]中长度为m的子字符串T[s+1...s+m]对应的d进制数，那么s是有效位移当且仅当ts=p。其中所有ts的计算可以在O(n-m+1)时间内完成，所以总的时间复杂度为O(m)+O(n-m+1)=O(n)，当然这是在理想情况下，即ts和p都不是很大，如果过大，每次算术运算的就不能在常数时间完成。于是采用一个补救措施，计算ts和p对一个合适的数q的模。但这会导致一些伪命中点，所以对于所有满足ts=p (mod q)的点，需从左到右比较P[1...m]和T[s+1...s+m]是否相同。不过由于排除了很多非法位移，相比蛮力法，运行效率会好很多。预处理时间O(m)，匹配时间在最坏情况下为O((n-m+1)m)，不过期望的匹配时间为O(n)。

# <span style="color: #ff0000;">KMP算法(1977)</span>

通过预计算一个前缀函数π，从而在比较过程中可以跳过很多无效位移。π[q]表示P的最长前缀同时又是P[1...q]的真后缀的长度。预处理时间O(m)，匹配时间O(n)。空间复杂度O(m)。

想看更详细的KMP讲解，可以看[《算法导论》](http://book.douban.com/subject/1885170/)或到[Matrix67](http://www.matrix67.com/blog/archives/115)那里去看看。

# <span class="Apple-style-span" style="color: #ff0000;">BM算法(1977)</span>

[这篇博文](http://www.cnblogs.com/dsky/archive/2012/05/04/2483190.html)对BM算法讲的很详细

[这还有论文](http://www.inf.fh-flensburg.de/lang/algorithmen/pattern/bmen.htm)

[BM算法wiki](http://en.wikipedia.org/wiki/Boyer%E2%80%93Moore_string_search_algorithm)

# <span style="color: #ff0000;">Sunday算法(1990)</span>

Horspool(1980)是BM的简化，改进了BM中的坏字符规则，丢弃了好后缀规则，而Sunday又是Horspool的改进

[http://www.inf.fh-flensburg.de/lang/algorithmen/pattern/sundayen.htm](http://www.inf.fh-flensburg.de/lang/algorithmen/pattern/sundayen.htm)

# <span style="color: #ff0000;">two-way算法(1991)</span>

strstr函数的实现使用的是two-way算法。

---------------------------------------------------------------------------------------------------------------

以上所有算法的分析、源码、实例动画等都可以在[http://www-igm.univ-mlv.fr/~lecroq/string/](http://www-igm.univ-mlv.fr/~lecroq/string/)这里找到，里面还包含了其他更多字符串匹配算法。

--------------------------------------------------------------------------------------

# <span style="color: #ff0000;">AC自动机算法(1991)</span>

前面几个算法都是用来处理单字符串匹配的，AC自动机是用来处理多字符串匹配的，也就是说模式串有多个。具体可以到[这里](http://hi.baidu.com/nialv7/item/ce1ce015d44a6ba7feded52d)看看。大概思想是trie树+失败指针。终于知道什么是[trie树](http://zh.wikipedia.org/zh/Trie)了，输入提示以及词频统计什么的，都是trie树的功劳了。

[http://en.wikipedia.org/wiki/Aho%E2%80%93Corasick_string_matching_algorithm](http://en.wikipedia.org/wiki/Aho%E2%80%93Corasick_string_matching_algorithm)

还有UNIX中的fgrep就是用的这个算法。
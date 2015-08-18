title: vector笔记
tags:
  - C
  - Notes
id: 1938
categories:
  - wordpress
  - index.php
  - csit
date: 2012-10-06 22:15:18
---

[c]
#include &lt;cstdio&gt;
#include &lt;vector&gt;

using namespace std;

int main()
{
    vector &lt;int&gt; a;
    printf(&quot;size:%d capacity:%d\n&quot;, a.size(), a.capacity());
    a.push_back(1);
    printf(&quot;size:%d capacity:%d\n&quot;, a.size(), a.capacity());
    a.push_back(1);
    printf(&quot;size:%d capacity:%d\n&quot;, a.size(), a.capacity());
    a.push_back(1);
    printf(&quot;size:%d capacity:%d\n&quot;, a.size(), a.capacity());
    a.push_back(1);
    printf(&quot;size:%d capacity:%d\n&quot;, a.size(), a.capacity());
    a.push_back(1);
    printf(&quot;size:%d capacity:%d\n&quot;, a.size(), a.capacity());
    a.push_back(1);
    printf(&quot;size:%d capacity:%d\n&quot;, a.size(), a.capacity());
    a.push_back(1);
    printf(&quot;size:%d capacity:%d\n&quot;, a.size(), a.capacity());
    a.push_back(1);
    printf(&quot;size:%d capacity:%d\n&quot;, a.size(), a.capacity());
    a.clear();
    printf(&quot;size:%d capacity:%d\n&quot;, a.size(), a.capacity());
}
[/c]

输出：
<pre>size:0 capacity:0
size:1 capacity:1
size:2 capacity:2
size:3 capacity:4
size:4 capacity:4
size:5 capacity:8
size:6 capacity:8
size:7 capacity:8
size:8 capacity:8
size:0 capacity:8</pre>
验证2个事实：

1.vector每次容量不够的时候，会申请一个双倍的内存空间，然后被旧的数据拷的新的地方，并释放旧内存。

2.vector在clear之后（或erase很多元素之后），并不会释放内存。
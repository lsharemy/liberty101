title: sizeof陷阱
tags:
  - C
id: 1953
categories:
  - wordpress
  - index.php
  - csit
date: 2012-10-21 21:42:35
---

以下程序会输出什么？

[c]
#include &lt;cstdio&gt;

using namespace std;

int main(void)
{
    int a = 1;
    printf(&quot;%d\n&quot;, sizeof(a++));
    printf(&quot;%d\n&quot;, a);
}
[/c]

如果你第一次看到，可能会认为输出是：
<pre>4
2</pre>
不过，程序的真正输出是：
<pre>4
1</pre>
为什么？这里有篇文章分析的比较全：

[http://dev.yesky.com/143/2563643.shtml](http://dev.yesky.com/143/2563643.shtml)
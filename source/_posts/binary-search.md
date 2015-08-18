title: 二分搜索
tags:
  - Algorithm
  - C
id: 1966
categories:
  - wordpress
  - index.php
  - csit
date: 2012-11-04 16:25:19
---

好吧，来贴个自己写的二分搜索代码（包括普通的二分、返回第一个出现位置的二分、以及返回最后一个出现位置的二分）：

[c]
#include &lt;cstdio&gt;
#include &lt;cstdlib&gt;

int bs_left(int *a, int n, int x)
{
    int l, h, m;
    l = -1;
    h = n;
    while(l+1 != h)
    {
        m = l + (h-l)/2;
        if (x &lt; a[m])
            h = m;
        else
            l = m;
    }
    if (l == -1 || a[l] != x) return -1;
    return l;
}

int bs_right(int *a, int n, int x)
{
    int l, h, m;
    l = -1;
    h = n;
    while(l+1 != h)
    {
        m = l + (h-l)/2;
        if (x &gt; a[m])
            l = m;
        else
            h = m;
    }
    if (h == n || a[h] != x) return -1;
    return h;
}

int bs(int *a, int n, int x)
{
    int l, h, m;
    l = 0;
    h = n-1;
    while(l &lt;= h)
    {
        m = l + (h-l)/2;
        if (x &lt; a[m])
            h = m-1;
        else if (x &gt; a[m])
            l = m+1;
        else
            return m;
    }
    return -1;
}

main()
{
    int n;
    int x;
    scanf(&quot;%d&quot;, &amp;n);
    int *a = (int*)malloc(sizeof(int)*n);
    for (int i = 0; i &lt; n; i++)
        scanf(&quot;%d&quot;, &amp;a[i]);
    scanf(&quot;%d&quot;, &amp;x);
    printf(&quot;any:%d\n&quot;, bs(a, n, x));
    printf(&quot;first:%d\n&quot;, bs_right(a, n, x));
    printf(&quot;last:%d\n&quot;, bs_left(a, n, x));
}
[/c]

在《C标准库》里看了stdlib.h中bsearch的实现，和我的bs是类似的，标准库也就“不过如此”了，忽忽
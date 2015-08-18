title: USACO - PROB Score Inflation
tags:
  - USACO
id: 1794
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-22 16:34:56
---

发现是个背包问题，想起昨天迅哥问我：背包九讲看了没有？我说还木有。于是今天就碰到背包问题了，去看了背包九讲个前2讲，于是发现这个是个完全背包问题，于是就敲了。

代码：<!--more-->
[c]
/*
ID: lmy0525
PROG: inflate
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
#include &lt;cmath&gt;
#include &lt;vector&gt;
#include &lt;cstring&gt;
#include &lt;iomanip&gt;
#include &lt;queue&gt;
using namespace std;

ofstream fout;
ifstream fin;

int weight[10000];
int cost[10000];
int f[10000];

int main()
{
    fout.open(&quot;inflate.out&quot;);
    fin.open(&quot;inflate.in&quot;);
    int M, N;
    fin &gt;&gt; M &gt;&gt; N;
    for (int i = 0; i &lt; N; i++)
    {
        fin &gt;&gt; weight[i] &gt;&gt; cost[i];
    }

    for (int i = 0; i &lt; N; i++)
    {
        for (int j = cost[i]; j &lt;= M; j++)
        {
            f[j] = max(f[j], f[j-cost[i]]+weight[i]);
        }
    }
    fout &lt;&lt; f[M] &lt;&lt; endl;
    return 0;
}
[/c]

标程1：
[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

#define MAXCAT 10000
#define MAXTIME 10000

int best[MAXTIME+1];

void
main(void)
{
    FILE *fin, *fout;
    int tmax, ncat;
    int i, j, m, p, t;

    fin = fopen(&quot;inflate.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;inflate.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    fscanf(fin, &quot;%d %d&quot;, &amp;tmax, &amp;ncat);

    for(i=0; i&lt;ncat; i++) {
	fscanf(fin, &quot;%d %d&quot;, &amp;p, &amp;t);
	for(j=0; j+t &lt;= tmax; j++)
	    if(best[j]+p &gt; best[j+t])
	    	best[j+t] = best[j]+p;
    }

	m = 0;
    for(i=0; i&lt;=tmax; i++)
	if(m &lt; best[i])
	    m = best[i];

    fprintf(fout, &quot;%d\n&quot;, m);
    exit(0);
}
[/c]

标程2：
[c]
#include &lt;fstream.h&gt;

ifstream fin(&quot;inflate.in&quot;);
ofstream fout(&quot;inflate.out&quot;);

const short maxm = 10010;
long best[maxm], m, n;

void
main()
{
    short i, j, len, pts;

    fin &gt;&gt; m &gt;&gt; n;

    for (j = 0; j &lt;= m; j++)
        best[j] = 0;

    for (i = 0; i &lt; n; i++) {
        fin &gt;&gt; pts &gt;&gt; len;
        for (j = len; j &lt;= m; j++)
            if (best[j-len] + pts &gt; best[j])
                best[j] = best[j-len] + pts;
    }
    fout &lt;&lt; best[m] &lt;&lt; endl; // This is always the highest total.
}
[/c]
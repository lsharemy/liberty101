title: USACO - PROB Money Systems
tags:
  - USACO
id: 1774
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-21 11:39:11
---

终于知道点降维DP了，f(k, n)表示用前k种货币表示n的方法数，于是f(k,n)=f(k-1, n) + f(k-1, n-coins[k]),前面一种情况对应不使用货币k，后面一种情况对应使用货币k。然后通过一个循环，就可以把前面一维给省了。

代码：<!--more-->
[c]
/*
ID: lmy0525
PROG: money
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
#include &lt;cmath&gt;
#include &lt;vector&gt;

using namespace std;

ofstream fout;
ifstream fin;

int coins[25];
long long dp[10001];

int main() {
    fout.open(&quot;money.out&quot;);
    fin.open(&quot;money.in&quot;);
    int V, N;
    fin &gt;&gt; V &gt;&gt; N;
    for (int i = 0; i &lt; V; i++)
    {
        fin &gt;&gt; coins[i];
    }
    dp[0] = 1;
    for (int i = 0; i &lt; V; i++)
    {
        for (int j = coins[i]; j &lt;= N; j++)
        {
            dp[j] += dp[j-coins[i]];
        }
    }        
    fout &lt;&lt; dp[N] &lt;&lt; endl;
    return 0;
}
[/c]

标程：
[c]
/*
PROG: money
ID: rsc001
*/

#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

#define MAXTOTAL 10000

long long nway[MAXTOTAL+1];

void
main(void)
{
	FILE *fin, *fout;
	int i, j, n, v, c;

	fin = fopen(&quot;money.in&quot;, &quot;r&quot;);
	fout = fopen(&quot;money.out&quot;, &quot;w&quot;);
	assert(fin != NULL &amp;&amp; fout != NULL);

	fscanf(fin, &quot;%d %d&quot;, &amp;v, &amp;n);

	nway[0] = 1;
	for(i=0; i&lt;v; i++) {
		fscanf(fin, &quot;%d&quot;, &amp;c);

		for(j=c; j&lt;=n; j++)
			nway[j] += nway[j-c];
	}

	fprintf(fout, &quot;%lld\n&quot;, nway[n]);
}
[/c]
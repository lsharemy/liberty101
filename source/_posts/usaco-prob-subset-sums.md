title: USACO - PROB Subset Sums
tags:
  - USACO
id: 1757
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-19 20:44:27
---

别人认为这么简单的一道DP，我竟然纠结这么久，看了N个题解，才似乎明白了一点。最后是看了个E文的题解才搞明白的：[http://vaibhav-mittal.blogspot.com/2011/07/usaco-section-22-subset-sums.html](http://vaibhav-mittal.blogspot.com/2011/07/usaco-section-22-subset-sums.html)

方程f(k, n) = f(k-1, n) + f(k-1, n-k)，f(k, n)表示{1, 2, ..., k}有多少个子集的和刚好为n。考虑使用k和不使用k两种情况，f(k, n)可以转换为f(k-1, n)和f(k-1, n-k)。初始状态为f(0, 0) = 1。

代码：<!--more-->

[c]
/*
ID: lmy0525
PROG: subset
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

long long dp[40][391];

int main() {
    fout.open(&quot;subset.out&quot;);
    fin.open(&quot;subset.in&quot;);
    int N;
    fin &gt;&gt; N;
    int s = (N+1)*N/2;
    if (s % 2 != 0)
    {
        fout &lt;&lt; 0 &lt;&lt; endl;
    }
    else
    {
        s &gt;&gt;= 1;
        dp[0][0] = 1;
        for (int i = 1; i &lt;= N; i++)
        {
            for (int j = 0; j &lt;= s; j++)
            {
                dp[i][j] = dp[i-1][j] + (j&gt;=i?dp[i-1][j-i]:0);
            }
        }
        fout &lt;&lt; dp[N][s]/2 &lt;&lt; endl;
    }
    return 0;
}
[/c]

标程1：
[c]
/* Calculate how many two-way partitions of {1, 2, ..., N} are
   even splits (the sums of the elements of both partition are equal) */

#include &lt;stdio.h&gt;
#include &lt;string.h&gt;

#define MAXSUM 637

unsigned int numsets[637][51];

int max;
unsigned int sum;

main(int argc, char **argv)
{
  int lv, lv2, lv3;
  int cnt;
  FILE *fin, *fout;

  fin = fopen (&quot;subset.in&quot;, &quot;r&quot;);
  fscanf(fin, &quot;%d&quot;, &amp;max);
  fclose (fin);
  fout = fopen(&quot;subset.out&quot;, &quot;w&quot;);

  if ((max % 4) == 1 || (max % 4) == 2) {
    fprintf (stderr, &quot;0\n&quot;);
    exit(1);
  }

  sum = max * (max+1) / 4;

    memset(numsets, 0, sizeof(numsets[0]));
    numsets[0][0] = 1;
    for (lv = 1; lv &lt; max; lv++) {
      for (lv2 = 0; lv2 &lt;= sum; lv2++)
        numsets[lv2][lv] = numsets[lv2][lv-1];
      for (lv2 = 0; lv2 &lt;= sum-lv; lv2++)
        numsets[lv2+lv][lv] += numsets[lv2][lv-1];
    }

    fprintf (fout, &quot;%u\n&quot;, numsets[sum][max-1]);
    fclose (fout);
  exit (0);
}
[/c]

标程2：
[c]
#include &lt;fstream&gt;
using namespace std;
const unsigned int MAX_SUM = 1024;
int n;
unsigned long long int dyn[MAX_SUM];
ifstream fin (&quot;subset.in&quot;);
ofstream fout (&quot;subset.out&quot;);

int main() {
    fin &gt;&gt; n;
    fin.close();
    int s = n*(n+1);
    if (s % 4) {
        fout &lt;&lt; 0 &lt;&lt; endl;
        fout.close ();
        return ;
    }
    s /= 4;
    int i, j;
    dyn [0] = 1;
    for (i = 1; i &lt;= n; i++)
        for (j = s; j &gt;= i; j--) 
            dyn[j] += dyn[j-i];
    fout &lt;&lt; (dyn[s]/2) &lt;&lt; endl;
    fout.close();
    return 0;
}
[/c]
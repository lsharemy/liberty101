title: USACO - PROB Cow Pedigrees
tags:
  - USACO
id: 1770
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-20 21:10:35
---

又一个DP，调了我好久，原来是small数组没算对，dp[n][k]表示n个节点层数刚好为k的数量，small[n][k]表示n个节点层数小于等于k的数量。

代码：<!--more-->
[c]
/*
ID: lmy0525
PROG: nocows
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

int dp[201][101];
int small[201][101];

int main() {
    fout.open(&quot;nocows.out&quot;);
    fin.open(&quot;nocows.in&quot;);
    int N, K;
    fin &gt;&gt; N &gt;&gt; K;

    for (int i = 1; i &lt;= K; i++) small[1][i] = 1;
    for (int k = 2; k &lt;= K; k++)
    {
        for (int n = 3; n &lt;= N; n++)
        {
            for (int i = 1; i &lt;= n-2; i += 2)
            {
                small[n][k] += small[i][k-1]*small[n-1-i][k-1];
                small[n][k] %= 9901;
            }
        }
    }

    dp[1][1] = 1;
    for (int k = 2; k &lt;= K; k++)
    {
        for (int n = 3; n &lt;= N; n += 2)
        {
            for (int i = 1; i &lt;= n-2; i += 2)
            {
                dp[n][k] += small[i][k-1]*dp[n-1-i][k-1]
                    + dp[i][k-1]*small[n-1-i][k-1]
                    - dp[i][k-1]*dp[n-1-i][k-1];
                dp[n][k] %= 9901;
            }

        }
    }
    fout &lt;&lt; dp[N][K] &lt;&lt; endl;
    return 0;
}
[/c]

标程：

[c]
#include &lt;cstdio&gt;
#include &lt;cstdlib&gt;
#include &lt;cassert&gt;
#define MOD 9901
using namespace std;

int table[101][202],N,K,c;
int smalltrees[101][202];

FILE *fin=fopen(&quot;nocows.in&quot;,&quot;r&quot;);
FILE *fout=fopen(&quot;nocows.out&quot;,&quot;w&quot;);

int main() {
    fscanf (fin,&quot;%d %d&quot;,&amp;N,&amp;K);
    table[1][1]=1;
    for (int i=2;i&lt;=K;i++) {
        for (int j=1;j&lt;=N;j+=2)
            for (int k=1;k&lt;=j-1-k;k+=2) {
                if (k!=j-1-k) c=2; else c=1;    
                table[i][j]+=c*(
                        smalltrees[i-2][k]*table[i-1][j-1-k]  // left subtree smaller than i-1
                        +table[i-1][k]*smalltrees[i-2][j-1-k]  // right smaller
                        +table[i-1][k]*table[i-1][j-1-k]);// both i-1
                table[i][j]%=MOD;
            }
        for (int k=0;k&lt;=N;k++) {          // we ensure that smalltrees[i-2][j] in
the next i
            smalltrees[i-1][k]+=table[i-1][k]+smalltrees[i-2][k]; // iteration
contains the number
            smalltrees[i-1][k]%=MOD;           // of trees smaller than i-1 and with
j nodes
        }
    }

    fprintf (fout,&quot;%d\n&quot;,table[K][N]);
    return 0;
}
[/c]
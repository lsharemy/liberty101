title: USACO - PROB Home on the Range
tags:
  - USACO
id: 1865
categories:
  - wordpress
  - index.php
  - csit
date: 2012-08-14 18:42:02
---

好吧，又一个DP

代码：<!--more-->

[c]
/*
ID: lmy0525
PROG: range
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
#include &lt;map&gt;
#include &lt;stack&gt;
using namespace std;

ofstream fout(&quot;range.out&quot;);
ifstream fin(&quot;range.in&quot;);

char field[250][251];
int l[250][250];
int u[250][250];
int big[250][250];
int cnt[251];

int main()
{
    int N;
    fin &gt;&gt; N;
    for (int i = 0; i &lt; N; i++)
        fin &gt;&gt; field[i];

    for (int i = 0; i &lt; N; i++) big[i][0] = l[i][0] = (field[i][0]=='0'?0:1);
    for (int j = 1; j &lt; N; j++)
    for (int i = 0; i &lt; N; i++) l[i][j] = (field[i][j]=='0'?0:l[i][j-1]+1);

    for (int i = 0; i &lt; N; i++) big[0][i] = u[0][i] = (field[0][i]=='0'?0:1);
    for (int j = 1; j &lt; N; j++)
    for (int i = 0; i &lt; N; i++) u[j][i] = (field[j][i]=='0'?0:u[j-1][i]+1);

    for (int i = 1; i &lt; N; i++)
    for (int j = 1; j &lt; N; j++)
    {
        big[i][j] = min(min(l[i][j], u[i][j]), big[i-1][j-1]+1);
    }

    for (int i = 0; i &lt; N; i++)
    {
        for (int j = 0; j &lt; N; j++)
        {
            if (big[i][j] &gt;= 2)
            {
                for (int k = 2; k &lt;= big[i][j]; k++)
                {
                    cnt[k]++;
                }
            }
        }
    }
    for (int i = 2; i &lt;= 250; i++)
    {
        if (cnt[i] &gt; 0)
        {
            fout &lt;&lt; i &lt;&lt; &quot; &quot; &lt;&lt; cnt[i] &lt;&lt; endl;
        }
    }
    return 0;
}
[/c]

标程1：

[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

#define MAXN 250

int goodsq[MAXN][MAXN];
int bigsq[MAXN][MAXN];
int tot[MAXN+1];

int
min(int a, int b)
{
    return a &lt; b ? a : b;
}

void
main(void)
{
    FILE *fin, *fout;
    int i, j, k, l, n, sz;

    fin = fopen(&quot;range.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;range.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    fscanf(fin, &quot;%d\n&quot;, &amp;n);

    for(i=0; i&lt;n; i++) {
	for(j=0; j&lt;n; j++)
	    goodsq[i][j] = (getc(fin) == '1');
	assert(getc(fin) == '\n');
    }

    /* calculate size of biggest square with lower right corner (i,j) */
    for(i=0; i&lt;n; i++) {
	for(j=0; j&lt;n; j++) {
	    for(k=i; k&gt;=0; k--)
		if(goodsq[k][j] == 0)
		    break;

	    for(l=j; l&gt;=0; l--)
		if(goodsq[i][l] == 0)
		    break;

	    sz = min(i-k, j-l);
	    if(i &gt; 0 &amp;&amp; j &gt; 0)
		sz = min(sz, bigsq[i-1][j-1]+1);

	    bigsq[i][j] = sz;
	}
    }

    /* now just count squares */
    for(i=0; i&lt;n; i++)
    for(j=0; j&lt;n; j++)
    for(k=2; k&lt;=bigsq[i][j]; k++)
	tot[k]++;

    for(i=2; i&lt;=n; i++)
	if(tot[i])
	    fprintf(fout, &quot;%d %d\n&quot;, i, tot[i]);

    exit(0);
}
[/c]

标程2：

[c]
#include &lt;fstream.h&gt;

ifstream fin(&quot;range.in&quot;);
ofstream fout(&quot;range.out&quot;);

const unsigned short maxn = 250 + 5;

unsigned short n;
char fieldpr;
unsigned short sq[maxn]; // biggest-square values
unsigned short sqpr;
unsigned short numsq[maxn]; // number of squares of each size

unsigned short
min3(unsigned short a, unsigned short b, unsigned short c)
{
	if ((a &lt;= b) &amp;&amp; (a &lt;= c))
		return a;
	else 
		return (b &lt;= c) ? b : c;
}

void
main()
{
	unsigned short r, c;
	unsigned short i;
	unsigned short tmp;

	fin &gt;&gt; n;

	for (c = 1; c &lt;= n; c++)
		sq[c] = 0;

	for (i = 2; i &lt;= n; i++)
		numsq[i] = 0;

	for (r = 1; r &lt;= n; r++)
	{
		sqpr = 0;
		sq[0] = 0;
		for (c = 1; c &lt;= n; c++)
		{
			fin &gt;&gt; fieldpr;
			if (!(fieldpr - '0'))
			{
				sqpr = sq[c];
				sq[c] = 0;
				continue;
			}

			// Only three values needed.
			tmp = 1 + min3(sq[c-1], sqpr, sq[c]);
			sqpr = sq[c];
			sq[c] = tmp;

			// Only count maximal squares, for now.
			if (sq[c] &gt;= 2)
				numsq[ sq[c] ]++;
		}
	}

	// Count all squares, not just maximal. 
	for (i = n-1; i &gt;= 2; i--)
		numsq[i] += numsq[i+1];

	for (i = 2; i &lt;= n &amp;&amp; numsq[i]; i++)
		fout &lt;&lt; i &lt;&lt; ' ' &lt;&lt; numsq[i] &lt;&lt; endl;
}
[/c]
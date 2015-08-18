title: USACO - PROB Stringsobits
tags:
  - USACO
id: 1814
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-23 19:45:53
---

又做到迅哥以前教我的这种题了

代码：<!--more-->
[c]
/*
ID: lmy0525
PROG: kimbits
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
using namespace std;

ofstream fout;
ifstream fin;

int cnt[32][32]; // cnt(n, l) n bits, l ones

void initcnt()
{
    for (int j = 0; j &lt;= 31; j ++)
    {
        cnt[0][j] = 1;
    }

    for (int i = 1; i &lt;= 31; i++)
    for (int j = 0; j &lt;= 31; j++)
    {
        if (j == 0) cnt[i][j] = 1;
        else cnt[i][j] = cnt[i-1][j-1] + cnt[i-1][j];
    }
}

void printbits(int n, int l, long long idx)
{
    if (n == 0) return;

    if (idx &lt; cnt[n-1][l])
    {
        fout &lt;&lt; &quot;0&quot;;
        printbits(n-1, l, idx);
    }
    else
    {
        fout &lt;&lt; &quot;1&quot;;
        printbits(n-1, l-1, idx-cnt[n-1][l]);
    }
}

int main()
{
    fout.open(&quot;kimbits.out&quot;);
    fin.open(&quot;kimbits.in&quot;);
    int N, L;
    long long I;
    fin &gt;&gt; N &gt;&gt; L &gt;&gt; I;
    initcnt();
    printbits(N, L, I-1);
    fout &lt;&lt; endl;
    return 0;
}
[/c]

标程：
[c]
/*
PROG: kimbits
ID: rsc001
*/

#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

FILE *fout;

/* calculate binomial coefficient (n choose k) */
double sizeofset[33][33];

void
initsizeofset(void)
{
	int i, j;

	for(j=0; j&lt;=32; j++)
		sizeofset[0][j] = 1;

	for(i=1; i&lt;=32; i++)
	for(j=0; j&lt;=32; j++)
		if(j == 0)
			sizeofset[i][j] = 1;
		else
			sizeofset[i][j] = sizeofset[i-1][j-1] + sizeofset[i-1][j];
}

void
printbits(int nbits, int nones, double index)
{
	double s;

	if(nbits == 0)
		return;

	s = sizeofset[nbits-1][nones];
	if(s &lt;= index) {
		fprintf(fout, &quot;1&quot;);
		printbits(nbits-1, nones-1, index-s);
	} else {
		fprintf(fout, &quot;0&quot;);
		printbits(nbits-1, nones, index);
	}
}

void
main(void)
{
	FILE *fin;
	int nbits, nones;
	double index;

	fin = fopen(&quot;kimbits.in&quot;, &quot;r&quot;);
	fout = fopen(&quot;kimbits.out&quot;, &quot;w&quot;);
	assert(fin != NULL &amp;&amp; fout != NULL);

	initsizeofset();
	fscanf(fin, &quot;%d %d %lf&quot;, &amp;nbits, &amp;nones, &amp;index);
	printbits(nbits, nones, index-1);
	fprintf(fout, &quot;\n&quot;);

	exit(0);
}
[/c]
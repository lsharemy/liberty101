title: USACO - PROB Humble Numbers
tags:
  - USACO
id: 1796
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-22 17:19:26
---

原来还有一种数叫丑数

代码：<!--more-->
[c]
/*
ID: lmy0525
PROG: humble 
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

int K, N;
int prime[100];
int humble[100000];
int pindex[100];
int nhumble;

int main()
{
    fout.open(&quot;humble.out&quot;);
    fin.open(&quot;humble.in&quot;);
    fin &gt;&gt; K &gt;&gt; N;
    for (int i = 0; i &lt; N; i++)
    {
        fin &gt;&gt; prime[i];
    }
    nhumble = 1;
    humble[0] = 1;

    int min, minp;
    while(nhumble &lt; N+1)
    {
        min = 0x7fffffff;
        minp = -1;
        for (int i = 0; i &lt; K; i++)
        {
            while ((double)prime[i]*humble[pindex[i]] &lt;= humble[nhumble-1]) pindex[i]++;

            if ((double)prime[i]*humble[pindex[i]] &lt; min)
            {
                min = prime[i]*humble[pindex[i]];
                minp = i;
            }
        }
        humble[nhumble++] = min;
        pindex[minp]++;
    }
    fout &lt;&lt; humble[nhumble-1] &lt;&lt; endl;
    return 0;
}
[/c]

标程：
[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;
#include &lt;ctype.h&gt;

#define MAXPRIME 100
#define MAXN 100000

long hum[MAXN+1];
int nhum;

int prime[MAXPRIME];
int pindex[MAXPRIME];
int nprime;

void
main(void)
{
    FILE *fin, *fout;
    int i, minp;
    long min;
    int n;

    fin = fopen(&quot;humble.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;humble.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    fscanf(fin, &quot;%d %d&quot;, &amp;nprime, &amp;n);
    for(i=0; i&lt;nprime; i++)
	fscanf(fin, &quot;%d&quot;, &amp;prime[i]);

    hum[nhum++] = 1;
    for(i=0; i&lt;nprime; i++)
	pindex[i] = 0;

    while(nhum &lt; n+1) {
	min = 0x7FFFFFFF;
	minp = -1;
	for(i=0; i&lt;nprime; i++) {
	    while((double)prime[i] * hum[pindex[i]] &lt;= hum[nhum-1]) 
		pindex[i]++;

	    /* double to avoid overflow problems */
	    if((double)prime[i] * hum[pindex[i]] &lt; min) {
		min = prime[i] * hum[pindex[i]];
		minp = i;
	    }
	}

	hum[nhum++] = min;
	pindex[minp]++;
    }

    fprintf(fout, &quot;%d\n&quot;, hum[n]);
    exit(0);
}
[/c]
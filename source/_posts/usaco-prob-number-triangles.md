title: USACO - PROB Number Triangles
tags:
  - USACO
id: 1703
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-16 18:30:02
---

我的：<!--more-->

[c]
/*
ID: lmy0525
PROG: numtri
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;

using namespace std;

int tri[1001][1001];
int sum[1001][1001] = {0};

int main() {
    ofstream fout (&quot;numtri.out&quot;);
    ifstream fin (&quot;numtri.in&quot;);
    int R;
    fin &gt;&gt; R;
    for (int i = 1; i &lt;= R; i++)
    {
        for (int j = 1; j &lt;= i; j++)
        {
            fin &gt;&gt; tri[i][j];
        }
    }
    for (int i = 1; i &lt;= R; i++)
    {
        for (int j = 1; j &lt;= i; j++)
        {
            sum[i][j] = tri[i][j] + max(sum[i-1][j-1], sum[i-1][j]);
        }
    }
    int largest = 0;
    for (int i = 1; i &lt;= R; i++)
        largest = max(largest, sum[R][i]);
    fout &lt;&lt; largest &lt;&lt; endl;
    return 0;
}
[/c]

空间复杂度可以缩小

标程：
[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

#define MAXR 1000

int
max(int a, int b)
{
	return a &gt; b ? a : b;
}

void
main(void)
{
	int best[MAXR], oldbest[MAXR];
	int i, j, r, n, m;
	FILE *fin, *fout;

	fin = fopen(&quot;numtri.in&quot;, &quot;r&quot;);
	assert(fin != NULL);
	fout = fopen(&quot;numtri.out&quot;, &quot;w&quot;);
	assert(fout != NULL);

	fscanf(fin, &quot;%d&quot;, &amp;r);

	for(i=0; i&lt;MAXR; i++)
		best[i] = 0;

	for(i=1; i&lt;=r; i++) {
		memmove(oldbest, best, sizeof oldbest);
		for(j=0; j&lt;i; j++) {
			fscanf(fin, &quot;%d&quot;, &amp;n);
			if(j == 0)
				best[j] = oldbest[j] + n;
			else
				best[j] = max(oldbest[j], oldbest[j-1]) + n;
		}
	}

	m = 0;
	for(i=0; i&lt;r; i++)
		if(best[i] &gt; m)
			m = best[i];

	fprintf(fout, &quot;%d\n&quot;, m);
	exit(0);
}
[/c]
title: USACO - PROB Factorials
tags:
  - USACO
id: 1812
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-23 16:41:34
---

我的：
<!--more-->
[c]
/*
ID: lmy0525
PROG: fact4
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

int calc(int n)
{
    if (n == 1) return 1;
    int temp = n;
    while (temp % 10 == 0) temp /= 10;
    temp %= 10000;
    int res = temp*calc(n-1);
    while (res % 10 == 0) res /= 10;
    return res%10000;
}

int main()
{
    fout.open(&quot;fact4.out&quot;);
    fin.open(&quot;fact4.in&quot;);
    int N;
    fin &gt;&gt; N;
    fout &lt;&lt; calc(N)%10 &lt;&lt; endl;
    return 0;
}
[/c]

标程：
[c]
/*
PROG: fact4
ID: rsc001
*/

#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

void
main(void)
{
	FILE *fin, *fout;
	int n2, n5, i, j, n, digit;

	fin = fopen(&quot;fact4.in&quot;, &quot;r&quot;);
	fout = fopen(&quot;fact4.out&quot;, &quot;w&quot;);
	assert(fin != NULL &amp;&amp; fout != NULL);

	fscanf(fin, &quot;%d&quot;, &amp;n);
	digit = 1;
	n2 = n5 = 0;
	for(i=2; i&lt;=n; i++) {
		j = i;
		while(j%2 == 0) {
			n2++;
			j /= 2;
		}
		while(j%5 == 0) {
			n5++;
			j /= 5;
		}
		digit = (digit * j) % 10;
	}

	for(i=0; i&lt;n2-n5; i++)
		digit = (digit * 2) % 10;

	fprintf(fout, &quot;%d\n&quot;, digit);
	exit(0);
}
[/c]
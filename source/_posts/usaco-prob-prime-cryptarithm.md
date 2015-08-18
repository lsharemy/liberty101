title: USACO - PROB Prime Cryptarithm
tags:
  - USACO
id: 1666
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-13 21:03:37
---

我的：<!--more-->

[c]
/*
ID: lmy0525
PROG: crypt1
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;

using namespace std;

int isgooddigit[10] = {0};

bool isgoodnum(int num, int d)
{
    int cnt = 0;
    while (num)
    {
        if(!isgooddigit[num%10]) return false;
        num /= 10;
        cnt++;
    }
    if (cnt != d) return false;
    return true;
}

bool issolution(int i, int j)
{
    if (!isgoodnum(i, 3) || !isgoodnum(j, 2) || !isgoodnum(i*j, 4)) return false;
    while (j)
    {
        if (!isgoodnum(i*(j%10), 3)) return false;
        j /= 10;
    }
    return true;
}

int main() {
    ofstream fout (&quot;crypt1.out&quot;);
    ifstream fin (&quot;crypt1.in&quot;);
    int N;
    fin &gt;&gt; N;
    for (int i = 0; i &lt; N; i++)
    {
        int digit;
        fin &gt;&gt; digit;
        isgooddigit[digit] = 1;
    }
    int res = 0;
    for (int i = 100; i &lt; 1000; i++)
    {
        for (int j = 10; j &lt; 100; j++)
        {
            if (issolution(i, j)) res++;
        }
    }
    fout &lt;&lt; res &lt;&lt; endl;
    return 0;
}
[/c]

标程：

[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

int isgooddigit[10];	/* isgooddigit[d] is set if d is an acceptable digit
*/

/* check that every decimal digit in &quot;n&quot; is a good digit,
   and that it has the right number &quot;d&quot; of digits. */
int
isgood(int n, int d)
{
    if(n == 0)
		return 0;

    while(n) {
	if(!isgooddigit[n%10])
	    return 0;
	n /= 10;
        d--;
    }

    if( d == 0 )
       return 1;
    else
       return 0;
}

/* check that every product line in n * m is an okay number */
int
isgoodprod(int n, int m)
{
    if(!isgood(n,3) || !isgood(m,2) || !isgood(n*m,4))
	return 0;

    while(m) {
	if(!isgood(n*(m%10),3))
	    return 0;
	m /= 10;
    }
    return 1;
}

void
main(void)
{
    int i, j, n, nfound;
    FILE *fin, *fout;

    fin = fopen(&quot;crypt1.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;crypt1.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    for(i=0; i&lt;10; i++) {
        isgooddigit[i] = 0;
    }
    fscanf(fin, &quot;%d&quot;, &amp;n);
    for(i=0; i&lt;n; i++) {
	fscanf(fin, &quot;%d&quot;, &amp;j);
	isgooddigit[j] = 1;
    }

   nfound = 0;
   for(i=100; i&lt;1000; i++)
	for(j=10; j&lt;100; j++)
	    if(isgoodprod(i, j))
		nfound++;

   fprintf(fout, &quot;%d\n&quot;, nfound);
   exit(0);
}
[/c]

这题不小心google了以下，还不小心瞄了一眼标程，于是，，，
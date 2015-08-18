title: USACO - PROB Mixing Milk
tags:
  - USACO
id: 1649
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-13 12:53:30
---

我的：<!--more-->

[c]
/*
ID: your_id_here
PROG: milk
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;vector&gt;
#include &lt;algorithm&gt;

using namespace std;

vector &lt; pair&lt;int, int&gt; &gt; farmers;

int main() {
    ofstream fout (&quot;milk.out&quot;);
    ifstream fin (&quot;milk.in&quot;);
    int N, M;
    fin &gt;&gt; N &gt;&gt; M;
    for (int i = 0; i &lt; M; i++)
    {
        int price, amount;
        fin &gt;&gt; price &gt;&gt; amount;
        farmers.push_back(make_pair(price, amount));
    }
    sort (farmers.begin(), farmers.end());
    int res = 0;
    for (int i = 0; i &lt; M; i++)
    {
        int price = farmers[i].first;
        int amount = farmers[i].second;
        if (amount &lt; N)
        {
            // buy all
            res += amount*price;
            N -= amount;
        }
        else
        {
            res += N*price;
            break;

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

#define MAXFARMER 5000

typedef struct Farmer Farmer;
struct Farmer {
	int p;	/* price per gallon */
	int a;	/* amount to sell */
};

int
farmcmp(const void *va, const void *vb)
{
	return ((Farmer*)va)-&gt;p - ((Farmer*)vb)-&gt;p;
}

int nfarmer;
Farmer farmer[MAXFARMER];

void
main(void)
{
	FILE *fin, *fout;
	int i, n, a, p;

	fin = fopen(&quot;milk.in&quot;, &quot;r&quot;);
	fout = fopen(&quot;milk.out&quot;, &quot;w&quot;);

	assert(fin != NULL &amp;&amp; fout != NULL);

	fscanf(fin, &quot;%d %d&quot;, &amp;n, &amp;nfarmer);
	for(i=0; i&lt;nfarmer; i++)
		fscanf(fin, &quot;%d %d&quot;, &amp;farmer[i].p, &amp;farmer[i].a);

	qsort(farmer, nfarmer, sizeof(farmer[0]), farmcmp);

	p = 0;
	for(i=0; i&lt;nfarmer &amp;&amp; n &gt; 0; i++) {
		/* take as much as possible from farmer[i], up to amount n */
		a = farmer[i].a;
		if(a &gt; n)
			a = n;
		p += a*farmer[i].p;
		n -= a;
	}

	fprintf(fout, &quot;%d\n&quot;, p);
	exit(0);
}
[/c]

一样的思路。

对于这个题，有更好的方法：

[c]
#include &lt;fstream&gt;
#define MAXPRICE 1001
using namespace std;

int main() {
    ifstream fin (&quot;milk.in&quot;);
    ofstream fout (&quot;milk.out&quot;);
    unsigned int i, needed, price, paid, farmers, amount, milk[MAXPRICE][2];
    paid = 0;
    fin&gt;&gt;needed&gt;&gt;farmers;
    for(i = 0;i&lt;farmers;i++){
        fin&gt;&gt;price&gt;&gt;amount;
        milk[price][0] += amount;
    }
    for(i = 0; i&lt;MAXPRICE &amp;&amp; needed;i++){
        if(needed&gt; = milk[i][0]) {
            needed -= milk[i][0];
            paid += milk[i][0] * i;
        } else if(milk[i][0]&gt;0) {
            paid += i*needed;
            needed = 0;
        }
    }
    fout &lt;&lt; paid &lt;&lt; endl;
    return 0;
}
[/c]

类似于计数排序的思想，将O(nlogn)变成O(n)。
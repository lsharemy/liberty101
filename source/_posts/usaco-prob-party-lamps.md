title: USACO - PROB Party Lamps
tags:
  - USACO
id: 1763
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-20 17:26:52
---

每个开关的2种状态，4个开关只有16种状态，所以，生成这16种状态，并一一检查合理性，最后排序输出。标程还利用了6个灯泡是一个循环节。

我的：<!--more-->
[c]
/*
ID: lmy0525
PROG: lamps
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

int N, C;
vector &lt;int&gt; on, off;
int status[4];
int d[4][2] = { {1, 1}, {1, 2}, {2, 2}, {1, 3} };
vector &lt;string&gt; res;

void generate (int cur)
{
    if (cur == 4)
    {
        int ones = 0;
        int lamps[101];
        for (int i = 1; i &lt;= N; i++) lamps[i] = 1;
        // turn the lamps
        for (int i = 0; i &lt; 4; i++)
        {
            if (status[i] == 1)
            {
                ones++;
                for (int j = d[i][0]; j &lt;= N; j += d[i][1])
                {
                    lamps[j] = 1-lamps[j];
                }
            }
        }
        // check config    
        for (int i = 0; i &lt; on.size(); i++)
        {
            if (lamps[on[i]] != 1) return;
        }
        for (int i = 0; i &lt; off.size(); i++)
        {
            if (lamps[off[i]] != 0) return;
        }
        // check C
        if (C &lt; ones) return;
        if (C%2 != ones%2) return;

        string temp = &quot;&quot;;
        for (int i = 1; i &lt;= N; i++)
            temp += lamps[i]+'0';
        res.push_back(temp);  
        return;
    }

    status[cur] = 0;
    generate(cur+1);

    status[cur] = 1;
    generate(cur+1);
}

int main() {
    fout.open(&quot;lamps.out&quot;);
    fin.open(&quot;lamps.in&quot;);
    fin &gt;&gt; N &gt;&gt; C;
    int temp;
    fin &gt;&gt; temp;
    while (temp != -1)
    {
        on.push_back(temp);
        fin &gt;&gt; temp;
    }
    fin &gt;&gt; temp;
    while (temp != -1)
    {
        off.push_back(temp);
        fin &gt;&gt; temp;
    }
    generate(0);
    sort(res.begin(), res.end());
    unique(res.begin(), res.end());
    for (int i = 0; i &lt; res.size(); i++)
        fout &lt;&lt; res[i] &lt;&lt; endl;
    if (res.size() == 0)
        fout &lt;&lt; &quot;IMPOSSIBLE&quot; &lt;&lt; endl;
    return 0;
}
[/c]

标程：
[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

#define MAXLAMP	6
#define LAMPMASK	((1&lt;&lt;MAXLAMP)-1)

int nlamp;
int nswitch;
int ison;
int known;

int poss[1&lt;&lt;MAXLAMP];

int flip[4] = {
    LAMPMASK,		/* flip all lights */
    LAMPMASK &amp; 0xAA, 	/* flip odd lights */
    LAMPMASK &amp; 0x55,	/* flip even lights */
    LAMPMASK &amp; ((1&lt;&lt;(MAXLAMP-1))|(1&lt;&lt;(MAXLAMP-4)))	/* lights 1, 4 */
};

/*
 * Starting with current light state ``lights'', flip exactly n switches
 * with number &gt;= i.
 */
void
search(int lights, int i, int n)
{
    if(n == 0) {
	if((lights &amp; known) == ison)
	    poss[lights] = 1;
	return;
    }

    for(; i&lt;4; i++)
	search(lights ^ flip[i], i+1, n-1);
}

void
printseq(FILE *fout, int lights)
{
    int i;
    char s[100+1];

    for(i=0; i&lt;nlamp; i++)
	s[i] = (lights &amp; (1&lt;&lt;(MAXLAMP-1 - i%MAXLAMP))) ? '1' : '0';
    s[nlamp] = '&#92;&#48;';
    fprintf(fout, &quot;%s\n&quot;, s);
}

void
main(void)
{
    FILE *fin, *fout;
    int a, i, impossible;

    fin = fopen(&quot;lamps.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;lamps.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    fscanf(fin, &quot;%d %d&quot;, &amp;nlamp, &amp;nswitch);

    for(;;) {
	fscanf(fin, &quot;%d&quot;, &amp;a);
	if(a == -1)
	    break;
	a = MAXLAMP-1 - (a-1) % MAXLAMP;
	ison |= 1&lt;&lt;a;
	known |= 1&lt;&lt;a;
    }

    for(;;) {
	fscanf(fin, &quot;%d&quot;, &amp;a);
	if(a == -1)
	    break;
	a = MAXLAMP-1 - (a-1) % MAXLAMP;
	assert((ison &amp; (1&lt;&lt;a)) == 0);
	known |= 1&lt;&lt;a;
    }

    if(nswitch &gt; 4)
	if(nswitch%2 == 0)
	    nswitch = 4;
	else
	    nswitch = 3;

    for(; nswitch &gt;= 0; nswitch -= 2)
	    search(LAMPMASK, 0, nswitch);

    impossible = 1;
    for(i=0; i&lt;(1&lt;&lt;MAXLAMP); i++) {
	if(poss[i]) {
	    printseq(fout, i);
	    impossible = 0;
	}
    }
    if(impossible)
	fprintf(fout, &quot;IMPOSSIBLE\n&quot;);

    exit(0);
}
[/c]
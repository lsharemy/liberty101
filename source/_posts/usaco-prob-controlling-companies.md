title: USACO - PROB Controlling Companies
tags:
  - USACO
id: 1777
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-21 14:45:58
---

这个题在TEXT Flood Fill Algorithms中分析过，用DFS搞定了

代码：<!--more-->
[c]
/*
ID: lmy0525
PROG: concom
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
#include &lt;cmath&gt;
#include &lt;vector&gt;
#include &lt;cstring&gt;

using namespace std;

ofstream fout;
ifstream fin;

vector &lt; pair&lt;int, int&gt; &gt; owns[101];
int per[101];
int color[101];

void dfs(int n)
{
    for (int i = 0; i &lt; owns[n].size(); i++)
    {
        int v = owns[n][i].first;
        int p = owns[n][i].second;
        per[v] += p;
        if (per[v] &gt; 50 &amp;&amp; color[v] == 0)
        {
            color[v] = 1;
            dfs(v);
        }
    }
}

int main()
{
    fout.open(&quot;concom.out&quot;);
    fin.open(&quot;concom.in&quot;);
    int n;
    fin &gt;&gt; n;
    for (int i = 0; i &lt; n; i++)
    {
        int a, b, p;
        fin &gt;&gt; a &gt;&gt; b &gt;&gt; p;
        owns[a].push_back(make_pair(b, p));
    }
    for (int i = 1; i &lt;= 100; i++)
    {
        memset(color, 0, sizeof(color));
        memset(per, 0, sizeof(per));
        dfs(i);
        for (int j = 1; j &lt;= 100; j++)
        {
            if (i == j) continue;
            if (per[j] &gt; 50) fout &lt;&lt; i &lt;&lt; &quot; &quot; &lt;&lt; j &lt;&lt; endl;
        }
    }
    return 0;
}
[/c]

标程：
[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

#define NCOM 101

int owns[NCOM][NCOM];        /* [i,j]: how much of j do i and its
                                controlled companies own? */
int controls[NCOM][NCOM];    /* [i, j]: does i control j? */

/* update info: now i controls j */
void
addcontroller(int i, int j)
{
    int k;

    if(controls[i][j])        /* already knew that */
        return;

    controls[i][j] = 1;

    /* now that i controls j, add to i's holdings everything from j */
    for(k=0; k&lt;NCOM; k++)
        owns[i][k] += owns[j][k];

    /* record the fact that controllers of i now control j */
    for(k=0; k&lt;NCOM; k++)
        if(controls[k][i])
            addcontroller(k, j);

    /* if i now controls more companies, record that fact */
    for(k=0; k&lt;NCOM; k++)
        if(owns[i][k] &gt; 50)
            addcontroller(i, k);
}

/* update info: i owns p% of j */
void
addowner(int i, int j, int p)
{
    int k;

    /* add p% of j to each controller of i */
    for(k=0; k&lt;NCOM; k++)
        if(controls[k][i])
            owns[k][j] += p;

    /* look for new controllers of j */
    for(k=0; k&lt;NCOM; k++)
        if(owns[k][j] &gt; 50)
            addcontroller(k, j);
}

void
main(void)
{
    FILE *fin, *fout;
    int i, j, n, a, b, p;

    fin = fopen(&quot;concom.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;concom.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    for(i=0; i&lt;NCOM; i++)
        controls[i][i] = 1;

    fscanf(fin, &quot;%d&quot;, &amp;n);
    for(i=0; i&lt;n; i++) {
        fscanf(fin, &quot;%d %d %d&quot;, &amp;a, &amp;b, &amp;p);
        addowner(a, b, p);
    }

    for(i=0; i&lt;NCOM; i++)
    for(j=0; j&lt;NCOM; j++)
        if(i != j &amp;&amp; controls[i][j])
            fprintf(fout, &quot;%d %d\n&quot;, i, j);
    exit(0);
}
[/c]
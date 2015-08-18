title: USACO - PROB Agri-Net
tags:
  - USACO
id: 1791
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-22 15:20:38
---

MST的“处女作”

代码：<!--more-->

[c]
/*
ID: lmy0525
PROG: agrinet
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

int weight[100][100];
int parent[100];
int key[100];
int intree[100];
int N;

int mst_prim(int r)
{
    int res = 0;
    for (int i = 0; i &lt; N; i++) key[i] = 1e9;
    key[r] = 0;
    priority_queue &lt; pair&lt;int, int&gt; &gt; Q;
    Q.push(make_pair(-key[r], r));
    while(!Q.empty())
    {
        int u = Q.top().second;
        Q.pop();
        if (intree[u]) continue;
        intree[u] = true;
        res += key[u];
        // cout &lt;&lt; u &lt;&lt; &quot; &quot; &lt;&lt; key[u] &lt;&lt; endl;
        for (int i = 0; i &lt; N; i++)
        {
            if (!intree[i] &amp;&amp; weight[u][i] &lt; key[i])
            {
                key[i] = weight[u][i];
                parent[i] = u;
                Q.push(make_pair(-key[i], i));
            }
        }
    }
    return res;
}

int main()
{
    fout.open(&quot;agrinet.out&quot;);
    fin.open(&quot;agrinet.in&quot;);
    fin &gt;&gt; N;
    for (int i = 0; i &lt; N; i++)
    {
        for (int j = 0; j &lt; N; j++)
        {
            fin &gt;&gt; weight[i][j];
        }
    }
    fout &lt;&lt; mst_prim(0) &lt;&lt; endl;
    return 0;
}
[/c]

标程1：
[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

#define MAXFARM	100

int nfarm;
int dist[MAXFARM][MAXFARM];
int isconn[MAXFARM];

void
main(void)
{
    FILE *fin, *fout;
    int i, j, nfarm, nnode, mindist, minnode, total;

    fin = fopen(&quot;agrinet.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;agrinet.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    fscanf(fin, &quot;%d&quot;, &amp;nfarm);
    for(i=0; i&lt;nfarm; i++)
    for(j=0; j&lt;nfarm; j++) 
	fscanf(fin, &quot;%d&quot;, &amp;dist[i][j]);

    total = 0;
    isconn[0] = 1;
    nnode = 1;
    for(isconn[0]=1, nnode=1; nnode &lt; nfarm; nnode++) {
	mindist = 0;
	for(i=0; i&lt;nfarm; i++)
	for(j=0; j&lt;nfarm; j++) {
	    if(dist[i][j] &amp;&amp; isconn[i] &amp;&amp; !isconn[j]) {
	    	if(mindist == 0 || dist[i][j] &lt; mindist) {
		    mindist = dist[i][j];
		    minnode = j;
		}
	    }
	}
	assert(mindist != 0);

	isconn[minnode] = 1;
	total += mindist;
    }

    fprintf(fout, &quot;%d\n&quot;, total);

    exit(0);
}
[/c]

标程2：
[c]
#include &lt;fstream.h&gt;
#include &lt;assert.h&gt;

const int BIG = 20000000;

int     n;
int     dist[1000][1000];
int     distToTree[1000];
bool    inTree[1000];

main ()
{
    ifstream filein (&quot;agrinet.in&quot;);
    filein &gt;&gt; n;
    for (int i = 0; i &lt; n; ++i) {
	for (int j = 0; j &lt; n; ++j) {
	    filein &gt;&gt; dist[i][j];
	}
	distToTree[i] = BIG;
	inTree[i] = false;
    }
    filein.close ();

    int     cost = 0;
    distToTree[0] = 0;

    for (int i = 0; i &lt; n; ++i) {
	int     best = -1;
	for (int j = 0; j &lt; n; ++j) {
	    if (!inTree[j]) {
		if (best == -1 || distToTree[best] &gt; distToTree[j]) {
		    best = j;
		}
	    }
	}
	assert (best != -1);
	assert (!inTree[best]);
	assert (distToTree[best] &lt; BIG);

	inTree[best] = true;
	cost += distToTree[best];
	distToTree[best] = 0;
	for (int j = 0; j &lt; n; ++j) {
	    if (distToTree[j] &gt; dist[best][j]) {
		distToTree[j] = dist[best][j];
		assert (!inTree[j]);
	    }
	}
    }
    ofstream fileout (&quot;agrinet.out&quot;);
    fileout &lt;&lt; cost &lt;&lt; endl;
    fileout.close ();
    exit (0);
}
[/c]
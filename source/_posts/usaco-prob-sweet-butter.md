title: USACO - PROB Sweet Butter
tags:
  - USACO
id: 1851
categories:
  - wordpress
  - index.php
  - csit
date: 2012-08-12 17:40:16
---

听YX大牛说，我这样写就是SPFA，那就算它是SPFA吧。

代码：<!--more-->

[c]
/*
ID: lmy0525
PROG: butter
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

int N, P, C;
vector &lt;int&gt; cows;
vector &lt; pair &lt;int, int&gt; &gt; adj[801];
int d[801][801];

void dijkstra(int s)
{
    d[s][s] = 0;
    priority_queue &lt; pair&lt;int, int&gt; &gt; Q;
    Q.push(make_pair(0, s));
    while(!Q.empty())
    {
        int u = Q.top().second;
        int du = -Q.top().first;
        Q.pop();
        if (du &gt; d[s][u]) continue;
        for (int i = 0; i &lt; (int)adj[u].size(); i++)
        {
            int v = adj[u][i].first;
            int uv = adj[u][i].second;
            if (d[s][v] &gt; d[s][u] + uv)
            {
                d[s][v] = d[s][u] + uv;
                Q.push(make_pair(-d[s][v], v));
            }
        }
    }
}

int main()
{
    fout.open(&quot;butter.out&quot;);
    fin.open(&quot;butter.in&quot;);
    fin &gt;&gt; N &gt;&gt; P &gt;&gt; C;
    for (int i = 0; i &lt; N; i++)
    {
        int temp;
        fin &gt;&gt; temp;
        cows.push_back(temp);
    }
    for (int i = 0; i &lt; C; i++)
    {
        int a, b, l;
        fin &gt;&gt; a &gt;&gt; b &gt;&gt; l;
        adj[a].push_back(make_pair(b, l));
        adj[b].push_back(make_pair(a, l));
    }
    memset(d, 0x50, sizeof(d));
    for (int i = 0; i &lt; N; i++)
    {
        dijkstra(cows[i]);
    }
    int res = 0x50505050;
    for (int i = 1; i &lt;= P; i++)
    {
        int sum = 0;
        for (int j = 0; j &lt; N; j++)
        {
            sum += d[cows[j]][i];
        }
        res = min(sum, res);
    }
    fout &lt;&lt; res &lt;&lt; endl;
    return 0;
}
[/c]

标程：
[c]
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;

const int BIG = 1000000000;

const int MAXV = 800;
const int MAXC = 500;
const int MAXE = 1450;

int cows;
int v,e;

int cow_pos[MAXC];
int degree[MAXV];
int con[MAXV][MAXV];
int cost[MAXV][MAXV];

int dist[MAXC][MAXV];

int heapsize;
int heap_id[MAXV];
int heap_val[MAXV];
int heap_lookup[MAXV];

bool validheap(void){
  for(int i = 0; i &lt; heapsize; ++i){
    if(!(0 &lt;= heap_id[i] &amp;&amp; heap_id[i] &lt; v)){
      return(false);
    }
    if(heap_lookup[heap_id[i]] != i){
      return(false);
    }
  }
  return(true);
}

void heap_swap(int i, int j){
  int s;

  s = heap_val[i];
  heap_val[i] = heap_val[j];
  heap_val[j] = s;

  heap_lookup[heap_id[i]] = j;

  heap_lookup[heap_id[j]] = i;

  s = heap_id[i];
  heap_id[i] = heap_id[j];
  heap_id[j] = s;

}

void heap_up(int i){
  if(i &gt; 0 &amp;&amp; heap_val[(i-1) / 2] &gt; heap_val[i]){
    heap_swap(i, (i-1)/2);
    heap_up((i-1)/2);
  }
}

void heap_down(int i){
  int a = 2*i+1;
  int b = 2*i+2;

  if(b &lt; heapsize){
    if(heap_val[b] &lt; heap_val[a] &amp;&amp; heap_val[b] &lt; heap_val[i]){
      heap_swap(i, b);
      heap_down(b);
      return;
    }
  }
  if(a &lt; heapsize &amp;&amp; heap_val[a] &lt; heap_val[i]){
    heap_swap(i, a);
    heap_down(a);
  }
}

int main(){

  FILE *filein = fopen(&quot;butter.in&quot;, &quot;r&quot;);
  fscanf(filein, &quot;%d %d %d&quot;, &amp;cows, &amp;v, &amp;e);
  for(int i = 0; i &lt; cows; ++i){
    fscanf(filein, &quot;%d&quot;, &amp;cow_pos[i]);
    --cow_pos[i];
  }
  for(int i = 0; i &lt; v; ++i){
    degree[i] = 0;
  }
  for(int i = 0; i &lt; e; ++i){
    int a,b,c;
    fscanf(filein, &quot;%d %d %d&quot;, &amp;a, &amp;b, &amp;c);
    --a;
    --b;

    con[a][degree[a]] = b;
    cost[a][degree[a]] = c;
    ++degree[a];

    con[b][degree[b]] = a;
    cost[b][degree[b]] = c;
    ++degree[b];

  }
  fclose(filein);

  for(int i = 0; i &lt; cows; ++i){
    heapsize = v;
    for(int j = 0; j &lt; v; ++j){
      heap_id[j] = j;
      heap_val[j] = BIG;
      heap_lookup[j] = j;
    }
    heap_val[cow_pos[i]] = 0;
    heap_up(cow_pos[i]);

    bool fixed[MAXV];
    memset(fixed, false, v);
    for(int j = 0; j &lt; v; ++j){
      int p = heap_id[0];
      dist[i][p] = heap_val[0];
      fixed[p] = true;
      heap_swap(0, heapsize-1);
      --heapsize;
      heap_down(0);

      for(int k = 0; k &lt; degree[p]; ++k){
	int q = con[p][k];
	if(!fixed[q]){
	  if(heap_val[heap_lookup[q]] &gt; dist[i][p] + cost[p][k]){
	    heap_val[heap_lookup[q]] = dist[i][p] + cost[p][k];
	    heap_up(heap_lookup[q]);
	  }
	}
      }

    }
  }

  int best = BIG;
  for(int i = 0; i &lt; v; ++i){
    int total = 0;
    for(int j = 0; j &lt; cows; ++j){
      total += dist[j][i];
    }
    best &lt;?= total;
  }

  FILE *fileout = fopen(&quot;butter.out&quot;, &quot;w&quot;);
  fprintf(fileout, &quot;%d\n&quot;, best);
  fclose(fileout);

  return(0);
}
[/c]
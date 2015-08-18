title: USACO - PROB Riding The Fences
tags:
  - USACO
id: 1853
categories:
  - wordpress
  - index.php
  - csit
date: 2012-08-13 14:26:55
---

第一次写欧拉路径/欧拉回路，之前的TEXT给了伪码

代码：<!--more-->

[c]
/*
ID: lmy0525
PROG: fence
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
#include &lt;stack&gt;
using namespace std;

ofstream fout;
ifstream fin;

int mat[501][501];
int degree[501];
stack &lt;int&gt; circuit;

void find_circuit(int node)
{
    if (degree[node] == 0)
    {
        circuit.push(node);
    }
    else
    {
        while(degree[node] &gt; 0)
        {
            int i;
            for (i = 1; i &lt;= 500; i++)
            {
                if (mat[node][i] &gt; 0)
                {
                    break;
                }
            }
            mat[node][i]--;
            mat[i][node]--;
            degree[node]--;
            degree[i]--;
            find_circuit(i);
        }
        circuit.push(node);
    }
}

int main()
{
    fout.open(&quot;fence.out&quot;);
    fin.open(&quot;fence.in&quot;);
    int F;
    fin &gt;&gt; F;
    for (int i = 0; i &lt; F; i++)
    {
        int a, b;
        fin &gt;&gt; a &gt;&gt; b;
        mat[a][b]++;
        mat[b][a]++;
        degree[a]++;
        degree[b]++;
    }
    bool done = false;
    for (int i = 1; i &lt;= 500; i++)
    {
        if (degree[i] % 2 == 1)
        {
            find_circuit(i);
            done = true;
            break;
        }
    }
    if (!done)
    {
        for (int i = 1; i &lt;= 500; i++)
        {
            if (degree[i] &gt; 0)
            {
                find_circuit(i);
                break;
            }
        }
    }
    while(!circuit.empty())
    {
        fout &lt;&lt; circuit.top() &lt;&lt; endl;
        circuit.pop();
    }
    return 0;
}
[/c]

标程：
[c]
/* Prob #5: Riding the Fences */
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;

#define MAXI 500
#define MAXF 1200
char conn[MAXI][MAXI];
int deg[MAXI];
int nconn;

int touched[MAXI];

int path[MAXF];
int plen;

/* Sanity check routine */
void fill(int loc)
 {
  int lv;

  touched[loc] = 1;
  for (lv = 0; lv &lt; nconn; lv++)
    if (conn[loc][lv] &amp;&amp; !touched[lv])
      fill(lv);
 }

/* Sanity check routine */
int is_connected(int st)
 {
  int lv;
  memset(touched, 0, sizeof(touched));
  fill(st);
  for (lv = 0; lv &lt; nconn; lv++)
    if (deg[lv] &amp;&amp; !touched[lv])
      return 0;
  return 1;
 }

/* this is exactly the Eulerian Path algorithm */
void find_path(int loc)
 {
  int lv;

  for (lv = 0; lv &lt; nconn; lv++)
    if (conn[loc][lv])
     {
      /* delete edge */
      conn[loc][lv]--;
      conn[lv][loc]--;
      deg[lv]--;
      deg[loc]--;

      /* find path from new location */
      find_path(lv);
     }

  /* add this node to the `end' of the path */
  path[plen++] = loc;
 }

int main(int argc, char **argv)
 {
  FILE *fin, *fout;
  int nfen;
  int lv;
  int x, y;

  if (argc == 1) 
   {
    if ((fin = fopen(&quot;fence.in&quot;, &quot;r&quot;)) == NULL)
     {
      perror (&quot;fopen fin&quot;);
      exit(1);
     }
    if ((fout = fopen(&quot;fence.out&quot;, &quot;w&quot;)) == NULL)
     {
      perror (&quot;fopen fout&quot;);
      exit(1);
     }
   } else {
    if ((fin = fopen(argv[1], &quot;r&quot;)) == NULL)
     {
      perror (&quot;fopen fin filename&quot;);
      exit(1);
     }
    fout = stdout;
   }

  fscanf (fin, &quot;%d&quot;, &amp;nfen);
  for (lv = 0; lv &lt; nfen; lv++)
   {
    fscanf (fin, &quot;%d %d&quot;, &amp;x, &amp;y);
    x--; y--;
    conn[x][y]++;
    conn[y][x]++;
    deg[x]++;
    deg[y]++;
    if (x &gt;= nconn) nconn = x+1;
    if (y &gt;= nconn) nconn = y+1;
   }

  /* find first node of odd degree */
  for (lv = 0; lv &lt; nconn; lv++)
    if (deg[lv] % 2 == 1) break;
  /* if no odd-degree node, find first node with non-zero degree */
  if (lv &gt;= nconn)
    for (lv = 0; lv &lt; nconn; lv++)
      if (deg[lv]) break;
#ifdef CHECKSANE
  if (!is_connected(lv)) /* input sanity check */
   {
    fprintf (stderr, &quot;Not connected?!?\n&quot;);
    return 0;
   }
#endif

  /* find the eulerian path */
  find_path(lv); 

  /* the path is discovered in reverse order */
  for (lv = plen-1; lv &gt;= 0; lv--)
    fprintf (fout, &quot;%i\n&quot;, path[lv]+1);
  return 0;
 }
[/c]
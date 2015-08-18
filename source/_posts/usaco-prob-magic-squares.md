title: USACO - PROB Magic Squares
tags:
  - USACO
id: 1846
categories:
  - wordpress
  - index.php
  - csit
date: 2012-08-12 15:55:51
---

回家了2周多，我又回来了。

可能太久没写程序了，BFS都写了这么久，刚开始准备开8^7的数组，但是总是超内存。于是用了传说中的康托展开。

用了在这里找到的encode/decode代码：[http://apps.topcoder.com/forums/?module=Thread&amp;threadID=579656](http://apps.topcoder.com/forums/?module=Thread&amp;threadID=579656)

代码：<!--more-->

[c]
/*
ID: lmy0525
PROG: msquare
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

vector&lt;int&gt; init(8);
vector&lt;int&gt; target(8);
int color[40320];
int d[40320];
char op[40320];
int p[40320];
vector &lt;int&gt; v(8);

void transA(vector&lt;int&gt; u)
{
    for (int i = 0; i &lt; 8; i++)
        v[i] = u[7-i];
}

void transB(vector&lt;int&gt; u)
{
    v[0] = u[3];
    v[1] = u[0];
    v[2] = u[1];
    v[3] = u[2];
    v[4] = u[5];
    v[5] = u[6];
    v[6] = u[7];
    v[7] = u[4];
}

void transC(vector&lt;int&gt; u)
{
    v[0] = u[0];
    v[1] = u[6];
    v[2] = u[1];
    v[3] = u[3];
    v[4] = u[4];
    v[5] = u[2];
    v[6] = u[5];
    v[7] = u[7];
}

bool same(vector&lt;int&gt; t, vector&lt;int&gt; u)
{
    for (int i = 0; i &lt; 8; i++)
        if (t[i] != u[i]) return false;
    return true;   
}

int prod[8] = {5040, 720, 120, 24, 6, 2, 1, 1};

vector &lt;int&gt; decode(int mask)
{
    vector &lt;int&gt; ret(8), t = init;
    int u, v;
    for(u = 0; u &lt; 8; u++)
    {
        v = mask / prod[u];
        ret[u] = t[v];
        t.erase(t.begin() + v);
        mask -= v * prod[u];
    }
    return ret;
}

int encode(vector &lt;int&gt; num)
{
    int u, v, mask = 0;
    vector &lt;int&gt; t = init;
    for(u = 0; u &lt; 8; u++)
    {
        for(v = 0; t[v] != num[u]; v++);
        mask += v * prod[u];
        t.erase(t.begin() + v);
    }
    return mask;
}

void bfs(vector&lt;int&gt; s)
{
    int temp = encode(s);
    color[temp] = 1;
    d[temp] = 0;
    p[temp] = -1;
    queue &lt; vector&lt;int&gt; &gt; Q;
    Q.push(s);
    while(!s.empty())
    {
        vector &lt;int&gt; u = Q.front();
        Q.pop();
        int tempu = encode(u);
        if (same(target, u))
        {
            temp = tempu;
            fout &lt;&lt; (int)d[temp] &lt;&lt; endl;
            string res = &quot;&quot;;
            while(p[temp] != -1)
            {
                res = op[temp] + res;
                u = decode(p[temp]);
                temp = encode(u);
            }
            int i;
            for (i = 0; i &lt; res.size(); i++)
            {
                if (i != 0 &amp;&amp; i % 60 == 0) fout &lt;&lt; endl;
                fout &lt;&lt; res[i];
            }
            fout &lt;&lt; endl;
            return;
        }
        transA(u);
        int tempv = encode(v);
        if (color[tempv] == 0)
        {
            color[tempv] = 1;
            d[tempv] = d[tempu] + 1;
            op[tempv] = 'A';
            p[tempv] = tempu;
            Q.push(v);
        }
        transB(u);
        tempv = encode(v);
        if (color[tempv] == 0)
        {
            color[tempv] = 1;
            d[tempv] = d[tempu] + 1;
            op[tempv] = 'B';
            p[tempv] = tempu;
            Q.push(v);
        }
        transC(u);
        tempv = encode(v);
        if (color[tempv] == 0)
        {
            color[tempv] = 1;
            d[tempv] = d[tempu] + 1;
            op[tempv] = 'C';
            p[tempv] = tempu;
            Q.push(v);
        }
    }
}

int main()
{
    fout.open(&quot;msquare.out&quot;);
    fin.open(&quot;msquare.in&quot;);
    for (int i = 0; i &lt; 8; i++)
        fin &gt;&gt; target[i];
    for (int i = 0; i &lt; 8; i++)
        init[i] = i+1;
    bfs(init);
    return 0;
}
[/c]

标程：
[c]
#include &lt;stdio.h&gt;
#include &lt;assert.h&gt;

/* the distance from the initial configuration (+1) */
/* dist == 0 =&gt; no know path */
int dist[40320];

/* calculate the index of a board */
int encode(int *board)
 {
  static int mult[8] = 
    {1, 8, 8*7, 8*7*6, 8*7*6*5,
     8*7*6*5*4, 8*7*6*5*4*3, 8*7*6*5*4*3*2};

  /* used to calculate the position of a number within the
     remaining ones */
  int look[8] = {0, 1, 2, 3, 4, 5, 6, 7};
  int rlook[8] = {0, 1, 2, 3, 4, 5, 6, 7};
  /* rlook[look[p]] = p and look[rlook[p]] = p */

  int lv, rv;
  int t;

  rv = 0;
  for (lv = 0; lv &lt; 8; lv++)
   {
    t = look[board[lv]]; /* the rank of the board position */
    assert(t &lt; 8-lv); /* sanity check */
    rv += t * mult[lv]; 

    assert(look[rlook[7-lv]] == 7-lv); /* sanity check */

    /* delete t */
    look[rlook[7-lv]] = t;
    rlook[t] = rlook[7-lv];
   }
  return rv;
 }

/* the set of transformations, in order */
static int tforms[3][8] = { {8, 7, 6, 5, 4, 3, 2, 1},
     {4, 1, 2, 3, 6, 7, 8, 5}, {1, 7, 2, 4, 5, 3, 6, 8} };

void do_trans(int *inboard, int *outboard, int t)
 { /* calculate the board (into outboard) that results from doing
      the t'th transformation to inboard */
  int lv;
  int *tform = tforms[t];

  assert(t &gt;= 0 &amp;&amp; t &lt; 3);

  for (lv = 0; lv &lt; 8; lv++)
    outboard[lv] = inboard[tform[lv]-1];
 }

void do_rtrans(int *inboard, int *outboard, int t)
 { /* calculate the board (into outboard) that which would result
      in inboard if the t'th transformation was applied to it */
  int lv;
  int *tform = tforms[t];

  assert(t &gt;= 0 &amp;&amp; t &lt; 3);

  for (lv = 0; lv &lt; 8; lv++)
    outboard[tform[lv]-1] = inboard[lv];
 }

/* queue for breadth-first search */
int queue[40325][8];
int qhead, qtail;

/* calculate the distance from each board to the ending board */
void do_dist(int *board)
 {
  int lv;
  int t1;
  int d, t;

  qhead = 0;
  qtail = 1;

  /* the ending board is 0 steps away from itself */
  for (lv = 0; lv &lt; 8; lv++) queue[0][lv] = board[lv];
  dist[encode(queue[0])] = 1; /* 0 steps (+ 1 offset for dist array) */

  while (qhead &lt; qtail)
   {
    t1 = encode(queue[qhead]);
    d = dist[t1];

    /* for each transformation */
    for (lv = 0; lv &lt; 3; lv++)
     {
      /* apply the reverse transformation */
      do_rtrans(queue[qhead], queue[qtail], lv);

      t = encode(queue[qtail]);
      if (dist[t] == 0) 
       { /* found a new board position!  add it to queue */
        qtail++;
        dist[t] = d+1;
       }
     }

    qhead++;
   }
 }

/* find the path from the initial configuration to the ending board */
void walk(FILE *fout)
 {
  int newboard[8];
  int cboard[8];
  int lv, lv2;
  int t, d;

  for (lv = 0; lv &lt; 8; lv++) cboard[lv] = lv;
  d = dist[encode(cboard)];
  /* start at the ending board */
  while (d &gt; 1)
   {
    for (lv = 0; lv &lt; 3; lv++)
     {
      do_trans(cboard, newboard, lv);
      t = encode(newboard);
      if (dist[t] == d-1) /* we found the previous board! */
       {
        /* output transformatino */
        fprintf (fout, &quot;%c&quot;, lv+'A');

	/* find the rest of the path */
        for (lv2 = 0; lv2 &lt; 8; lv2++) cboard[lv2] = newboard[lv2];
	break;
       }
     }
    assert(lv &lt; 3);
    d--;
   }
  fprintf (fout, &quot;\n&quot;);
 }

int main(int argc, char **argv)
 {
  FILE *fout, *fin;
  int board[8];
  int lv;

  if ((fin = fopen(&quot;msquare.in&quot;, &quot;r&quot;)) == NULL)
   {
    perror (&quot;fopen fin&quot;);
    exit(1);
   }
  if ((fout = fopen(&quot;msquare.out&quot;, &quot;w&quot;)) == NULL)
   {
    perror (&quot;fopen fout&quot;);
    exit(1);
   }

  for (lv = 0; lv &lt; 8; lv++) 
   {
    fscanf (fin, &quot;%d&quot;, &amp;board[lv]);
    board[lv]--; /* use 0-based instead of 1-based */
   }

  /* calculate the distance from each board to the ending board */
  do_dist(board);

  for (lv = 0; lv &lt; 8; lv++) board[lv] = lv;

  /* output the distance from and the path from the initial configuration */
  fprintf (fout, &quot;%d\n&quot;, dist[encode(board)]-1);
  walk(fout);

  return 0;
 }
[/c]
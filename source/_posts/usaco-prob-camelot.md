title: USACO - PROB Camelot
tags:
  - USACO
id: 1860
categories:
  - wordpress
  - index.php
  - csit
date: 2012-08-14 14:58:40
---

好吧，又去搜题解了，不过这个题的数据好像有更新，第20组数据是新的，网上很多代码都过不了这个数据。

这题要先用bfs计算最短路，然后枚举所有汇聚点。对于每一个汇聚点，要考虑king自己走过去和某一个knight去接king的情况。对于某一个knight去接king的情况，又要枚举它们碰面的地方，网上很多代码都是枚举king的+-1范围，其实需要枚举+-2的，反例：

8 8
D 5
B 1
F 1
B 3

最优解是在D2点会合，一共5步，king先走2步到B3和B3的knight会合，之后3个knight都只需1步，即可到D2。

代码：<!--more-->

[c]
/*
ID: lmy0525
PROG: camelot
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

ofstream fout(&quot;camelot.out&quot;);
ifstream fin(&quot;camelot.in&quot;);

int king[2];
vector &lt; pair&lt;int, int&gt; &gt; knight;
int R, C;
int color[26][30];
int d[26][30][26][30];

int step[8][2] = { {1,2},{2,1},{2,-1},{1,-2},{-1,-2},{-2,-1},{-2,1},{-1,2} };

void bfs(int x, int y)
{
    for (int i = 0; i &lt; C; i++)
    for (int j = 0; j &lt; R; j++)
    {
        color[i][j] = 0;
        d[x][y][i][j] = 9999999;
    }
    color[x][y] = 1;
    d[x][y][x][y] = 0;
    queue &lt; pair &lt;int, int&gt; &gt; Q;
    Q.push(make_pair(x, y));
    while(!Q.empty())
    {
        int ux = Q.front().first;
        int uy = Q.front().second;
        Q.pop();
        for (int i = 0; i &lt; 8; i++)
        {
            int vx = ux + step[i][0];
            int vy = uy + step[i][1];
            if ( vx &lt; 0 || vx &gt;= C || vy &lt; 0 || vy &gt;= R) continue;
            if (color[vx][vy] == 0)
            {
                color[vx][vy] = 1;
                d[x][y][vx][vy] = d[x][y][ux][uy] + 1;
                Q.push(make_pair(vx, vy));
            }
        }
    }
}

int calc(int x, int y)
{
    int sum = 0;
    for (int i = 0; i &lt; knight.size(); i++)
        sum += d[knight[i].first][knight[i].second][x][y];
    // king alone    
    int res = sum + max(abs(king[0]-x), abs(king[1]-y));
    // pick up
    for (int tx = max(0, king[0]-2); tx &lt;= min(C-1, king[0]+2); tx++)
    for (int ty = max(0, king[1]-2); ty &lt;= min(R-1, king[1]+2); ty++)
    for (int i = 0; i &lt; knight.size(); i++)
        res = min(res, sum - d[knight[i].first][knight[i].second][x][y] + d[knight[i].first][knight[i].second][tx][ty] + d[tx][ty][x][y] + max(abs(king[0]-tx), abs(king[1]-ty)));
    return res;    
}

int main()
{
    fin &gt;&gt; R &gt;&gt; C;
    char tempc;
    int tempi;
    fin &gt;&gt; tempc &gt;&gt; tempi;
    king[0] = tempc-'A';
    king[1] = tempi-1;
    while(fin &gt;&gt; tempc)
    {
        fin &gt;&gt; tempi;
        knight.push_back(make_pair(tempc-'A', tempi-1));
    }
    // calc the shortest path
    for (int i = 0; i &lt; C; i++)
    for (int j = 0; j &lt; R; j++)
        bfs(i, j);
    // calc the min move
    int res = 1e9;
    for (int i = 0; i &lt; C; i++)
    for (int j = 0; j &lt; R; j++)
        res = min(res, calc(i, j));
    fout &lt;&lt; res &lt;&lt; endl;    
    return 0;
}
[/c]

标程：
[c]
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;
#include &lt;stdlib.h&gt;

/* &quot;infinity&quot;... &gt; maximum distance possible (for one knight) */
#define MAXN 10400

/* maximum number of rows */
#define MAXR 40 

/* maximum number of columns */
#define MAXC 26 

/* cost of collecting all knights here */
int cost[MAXC][MAXR]; 

/* cost of getting a knight to collect the king */
int kingcost[MAXC][MAXR];

/* distance the king must travel to get to this position */
int kdist[MAXC][MAXR];

/* distance to get for current knight to get to this square */
/* third index: 0 =&gt; without king, 1 =&gt; with king */
int dist[MAXC][MAXR][2]; 

/* number of rows and columns */
int nrow, ncol;

int do_step(int x, int y, int kflag) {
    int f = 0; /* maximum distance added */
    int d = dist[x][y][kflag]; /* distance of current move */

  /* go through all possible moves that a knight can make */
    if (y &gt; 0) {
        if (x &gt; 1)
             if (dist[x-2][y-1][kflag] &gt; d+1) {
                 dist[x-2][y-1][kflag] = d+1;
                 f = 1;
             }
            if (x &lt; ncol-2) {
                if (dist[x+2][y-1][kflag] &gt; d+1) {
	            dist[x+2][y-1][kflag] = d+1;
	            f = 1;
	        }
            }
            if (y &gt; 1) {
                if (x &gt; 0)
	            if (dist[x-1][y-2][kflag] &gt; d+1) {
	                dist[x-1][y-2][kflag] = d+1;
	                f = 1;
	            }
	        if (x &lt; ncol-1)
	            if (dist[x+1][y-2][kflag] &gt; d+1) {
	                dist[x+1][y-2][kflag] = d+1;
	                f = 1;
	            }
            }
    }
    if (y &lt; nrow-1) {
        if (x &gt; 1)
            if (dist[x-2][y+1][kflag] &gt; d+1) {
                dist[x-2][y+1][kflag] = d+1;
                f = 1;
            }
            if (x &lt; ncol-2) {
                if (dist[x+2][y+1][kflag] &gt; d+1) {
                    dist[x+2][y+1][kflag] = d+1;
                    f = 1;
                }
            }
        if (y &lt; nrow-2) {
            if (x &gt; 0)
                if (dist[x-1][y+2][kflag] &gt; d+1) {
                    dist[x-1][y+2][kflag] = d+1;
                    f = 1;
                }
            if (x &lt; ncol-1)
                if (dist[x+1][y+2][kflag] &gt; d+1) {
                    dist[x+1][y+2][kflag] = d+1;
                    f = 1;
                }
        }
    }

/* also check the 'pick up king here' move */
    if (kflag == 0 &amp;&amp; dist[x][y][1] &gt; d + kdist[x][y]) {
        dist[x][y][1] = d + kdist[x][y];
        if (kdist[x][y] &gt; f) f = kdist[x][y];
    }
    return f; /* 1 if simple knight move made, 0 if no new move found */
}

void calc_dist(int col, int row) {
    int lv, lv2;	/* loop variables */
    int d;		/* current distance being checked */
    int max; 		/* maximum finite distance found so far */
    int f; 		/* temporary variable (returned value from do_step */

/* initiate all positions to be infinite distance away */
    for (lv = 0; lv &lt; ncol; lv++)
        for (lv2 = 0; lv2 &lt; nrow; lv2++)
            dist[lv][lv2][0] = dist[lv][lv2][1] = MAXN;

/* starting location is zero w/o king, kdist[col][row] with king */
    dist[col][row][0] = 0;
    max = dist[col][row][1] = kdist[col][row];

    for (d = 0; d &lt;= max; d++) { /* for each distance away */
        for (lv = 0; lv &lt; ncol; lv++)
            for (lv2 = 0; lv2 &lt; nrow; lv2++) {
				/* for each position that distance away */
                if (dist[lv][lv2][0] == d) {
				 /* update with moves through this square */
                    f = do_step(lv, lv2, 0);
                    if (d + f &gt; max)     /* update max if necessary */
			max = d + f;
                 }

                 if (dist[lv][lv2][1] == d) {
			/* same as above, except this time knight has king */
                     f = do_step(lv, lv2, 1);
                     if (d + f &gt; max) max = d + f;
                 }
            }
    }
}

int main(int argc, char **argv) {
    FILE *fout, *fin;
    char t[10];
    int pr, pc;
    int lv, lv2;
    int i, j;

    if ((fin = fopen(&quot;camelot.in&quot;, &quot;r&quot;)) == NULL) {
        perror (&quot;fopen fin&quot;);
        exit(1);
    }
    if ((fout = fopen(&quot;camelot.out&quot;, &quot;w&quot;)) == NULL) {
        perror (&quot;fopen fout&quot;);
        exit(1);
    }

    fscanf (fin, &quot;%d %d&quot;, &amp;nrow, &amp;ncol);
    fscanf (fin, &quot;%s %d&quot;, t, &amp;pr);
    pc = t[0] - 'A';
    pr--;

  /* Calculate cost of moving king from starting position to
   * each board position.  This is just the taxi-cab distance */
   for (lv = 0; lv &lt; ncol; lv++)
       for (lv2 = 0; lv2 &lt; nrow; lv2++) {
           i = abs(pc-lv);
           j = abs(pr-lv2);
           if (i &lt; j) i = j;
           kingcost[lv][lv2] = kdist[lv][lv2] = i;
       }

    while (fscanf (fin, &quot;%s %d&quot;, t, &amp;pr) == 2) { /* for all knights */
        pc = t[0] - 'A';
        pr--;

        /* calculate distances */
        calc_dist(pc, pr);

        for (lv = 0; lv &lt; ncol; lv++)
            for (lv2 = 0; lv2 &lt; nrow; lv2++) {
                /* to collect here, we must also move knight here */
                cost[lv][lv2] += dist[lv][lv2][0];

	        /* check to see if it's cheaper for the new knight to
	           pick the king up instead of whoever is doing it now */
	        if (dist[lv][lv2][1] - dist[lv][lv2][0] &lt; kingcost[lv][lv2]) {
	            kingcost[lv][lv2] = dist[lv][lv2][1] - dist[lv][lv2][0];
	        }
            }
    }
    /* find best square to collect in */
    pc = cost[0][0] + kingcost[0][0];

    for (lv = 0; lv &lt; ncol; lv++)
        for (lv2 = 0; lv2 &lt; nrow; lv2++)
            if (cost[lv][lv2] + kingcost[lv][lv2] &lt; pc) /* better square? */
                pc = cost[lv][lv2] + kingcost[lv][lv2]; 
  fprintf (fout, &quot;%i\n&quot;, pc);
  return 0;
}
[/c]
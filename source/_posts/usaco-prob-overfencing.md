title: USACO - PROB Overfencing
tags:
  - USACO
id: 1782
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-21 17:05:17
---

在两个出口出进行2次BFS，记录下每个点离最近出口的距离，最后找到一个离出口最远的点的距离。

代码：<!--more-->
[c]
/*
ID: lmy0525
PROG: maze1
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
#include &lt;cmath&gt;
#include &lt;vector&gt;
#include &lt;cstring&gt;
#include &lt;queue&gt;

using namespace std;

ofstream fout;
ifstream fin;

string maze[201];

int W, H;
int lines, width;
int color[201][77];
int d[201][77];
int dir[4][2] = { {-2, 0}, {2, 0}, {0, -2}, {0, 2} };
int out[201][77];

void bfs (int r, int c)
{
    color[r][c] = 1;
    d[r][c] = 1;
    out[r][c] = min(out[r][c], d[r][c]);

    queue &lt;int&gt; Qr;
    queue &lt;int&gt; Qc;
    Qr.push(r);
    Qc.push(c);
    while(!Qr.empty())
    {
        int ur = Qr.front();
        int uc = Qc.front();
        Qr.pop();
        Qc.pop();
        for (int i = 0; i &lt; 4; i++)
        {
            int vr = ur + dir[i][0];
            int vc = uc + dir[i][1];
            if (vr &lt; 0 || vr &gt; lines-1 || vc &lt; 0 || vc &gt; width-1) continue;
            if (maze[(ur+vr)/2][(uc+vc)/2] != ' ') continue;
            if (color[vr][vc] == 0)
            {
                color[vr][vc] = 1;
                d[vr][vc] = d[ur][uc] + 1;
                out[vr][vc] = min(out[vr][vc], d[vr][vc]);
                Qr.push(vr);
                Qc.push(vc);
            }
        }
    }
}

int main()
{
    fout.open(&quot;maze1.out&quot;);
    fin.open(&quot;maze1.in&quot;);
    fin &gt;&gt; W &gt;&gt; H;
    lines = 2*H+1;
    width = 2*W+1;
    string temp;
    getline(fin, temp); // read the \n
    for (int i = 0; i &lt; lines; i++)
    {
        getline(fin, maze[i]);
    }
    memset(out, 0x25252525, sizeof(out));
    for (int i = 1; i &lt; width; i += 2)
    {
        if (maze[0][i] == ' ') // N
        {
            memset(color, 0, sizeof(color));
            memset(d, 0, sizeof(d));
            bfs(1, i);
        }
        if (maze[lines-1][i] == ' ') // S
        {
            memset(color, 0, sizeof(color));
            memset(d, 0, sizeof(d));
            bfs(lines-2, i);
        }
    }
    for (int i = 1; i &lt; lines; i += 2)
    {
        if (maze[i][0] == ' ') // W 
        {
            memset(color, 0, sizeof(color));
            memset(d, 0, sizeof(d));
            bfs(i, 1);
        }
        if (maze[i][width-1] == ' ') // E
        {
            memset(color, 0, sizeof(color));
            memset(d, 0, sizeof(d));
            bfs(i, width-2);
        }
    }
    int res = 0;
    for (int i = 1; i &lt; lines; i += 2)
    {
        for (int j = 1; j &lt; width; j += 2)
        {
            res = max(res, out[i][j]);
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

#define MAXWID 38
#define MAXHT 100

typedef struct Point Point;
struct Point {
	int r, c;
};

int wid, ht;
char maze[MAXHT*2+1][MAXWID*2+1+2];	/* extra +2 for &quot;\n&#92;&#48;&quot; */
int dist[MAXHT*2+1][MAXWID*2+1];

Point
Pt(int r, int c)
{
    Point p;

    p.r = r;
    p.c = c;
    return p;
}

typedef struct Queue Queue;
struct Queue {
    Point p;
    int d;
};

Queue floodq[MAXHT*MAXWID];
int bq, eq;

/* if no wall between point p and point np, add np to queue with distance d+1 */
void
addqueue(int d, Point p, Point np)
{
    if(maze[(p.r+np.r)/2][(p.c+np.c)/2] == ' ' &amp;&amp; maze[np.r][np.c] == ' ') {
	maze[np.r][np.c] = '*';
	floodq[eq].p = np;
	floodq[eq].d = d+1;
	eq++;
    }
}

/* if there is an exit at point exitp, plug it and record a start point
 * at startp */

void
lookexit(Point exitp, Point startp)
{
    if(maze[exitp.r][exitp.c] == ' ') {
	addqueue(0, startp, startp);
	maze[exitp.r][exitp.c] = '#';
    }
}

void
main(void)
{
    FILE *fin, *fout;
    Point p;
    int i, r, c, m, d;

    fin = fopen(&quot;maze1.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;maze1.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    fscanf(fin, &quot;%d %d\n&quot;, &amp;wid, &amp;ht);
    wid = 2*wid+1;
    ht = 2*ht+1;

    for(i=0; i&lt;ht; i++)
	fgets(maze[i], sizeof(maze[i]), fin);

    /* find exits */
    for(i=1; i&lt;wid; i+=2) {
	lookexit(Pt(0, i), Pt(1, i));
	lookexit(Pt(ht-1, i), Pt(ht-2, i));
    }
    for(i=1; i&lt;ht; i+=2) {
	lookexit(Pt(i, 0), Pt(i, 1));
	lookexit(Pt(i, wid-1), Pt(i, wid-2));
    }

    /* must have found at least one square with an exit */
    /* since two exits might open onto the same square, perhaps eq == 1 */
    assert(eq == 1 || eq == 2);	

    for(bq = 0; bq &lt; eq; bq++) {
	p = floodq[bq].p;
	d = floodq[bq].d;
	dist[p.r][p.c] = d;

	addqueue(d, p, Pt(p.r-2, p.c));
	addqueue(d, p, Pt(p.r+2, p.c));
	addqueue(d, p, Pt(p.r, p.c-2));
	addqueue(d, p, Pt(p.r, p.c+2));
    }

    /* find maximum distance */
    m = 0;
    for(r=0; r&lt;ht; r++)
    for(c=0; c&lt;wid; c++)
	if(dist[r][c] &gt; m)
	    m = dist[r][c];

    fprintf(fout, &quot;%d\n&quot;, m);

    exit(0);
}
[/c]
title: USACO - PROB Cow Tours
tags:
  - USACO
id: 1784
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-21 20:23:56
---

第一次看到这个题的时候，感觉好麻烦噢，不过把思路理清之后还好啦。这次使用了之前TEXT Winning Solutions中提到的先写注释再敲代码的方法，好像还真有点用，忽忽。

代码：<!--more-->

[c]
/*
ID: lmy0525
PROG: cowtour
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

using namespace std;

ofstream fout;
ifstream fin;

#define INF 1e9

typedef struct Points
{
    int x;
    int y;
}Points;

int N;
Points pts[150];
string matrix[150];
double dist[150][150];
int color[150];
double farest[150];
double diam[150];

double getdist(Points p1, Points p2)
{
    return sqrt((p1.x-p2.x)*(p1.x-p2.x) + (p1.y-p2.y)*(p1.y-p2.y));
}

void mark(int n, int c)
{
    color[n] = c;
    for (int i = 0; i &lt; N; i++)
    {
        if (dist[n][i] != INF &amp;&amp; color[i] == 0) mark(i, c);
    }
}

int main()
{
    fout.open(&quot;cowtour.out&quot;);
    fin.open(&quot;cowtour.in&quot;);
    // input
    fin &gt;&gt; N;
    for (int i = 0; i &lt; N; i++)
    {
        fin &gt;&gt; pts[i].x &gt;&gt; pts[i].y;
    }
    for (int i = 0; i &lt; N; i++)
    {
        fin &gt;&gt; matrix[i];
    }
    // precalc
    for (int i = 0; i &lt; N; i++)
    {
        for (int j = 0; j &lt; N; j++)
        {
            if (i == j)
            {
                dist[i][j] = 0;
            }
            else if (matrix[i][j] == '0')
            {
                dist[i][j] = INF;
            }
            else if (matrix[i][j] == '1')
            {
                dist[i][j] = getdist(pts[i], pts[j]);
            }
        }
    }
    // floyd
    for (int k = 0; k &lt; N; k++)
    for (int i = 0; i &lt; N; i++)
    for (int j = 0; j &lt; N; j++)
    {
        if (dist[i][k] + dist[k][j] &lt; dist[i][j])
            dist[i][j] = dist[i][k] + dist[k][j];
    }
    // mark
    int cur = 1;
    for (int i = 0; i &lt; N; i++)
    {
        if (color[i] == 0) mark(i, cur++);
    }
    // diam &amp; farest
    for (int i = 0; i &lt; N; i++)
    {
        for (int j = 0; j &lt; N; j++)
        {
             if (i == j) continue;
             if (dist[i][j] != INF)
             {
                 farest[i] = max(farest[i], dist[i][j]);
                 diam[color[i]] = max(diam[color[i]], dist[i][j]);
             }
        }
    }
    double res = 1e9;    
    // connect
    for (int i = 0; i &lt; N; i++)
    {
        for (int j = 0; j &lt; N; j++)
        {
            if (color[i] != color[j])
            {
                double newdiam = max(farest[i]+farest[j]+getdist(pts[i], pts[j]), max(diam[color[i]], diam[color[j]]));
                res = min(res, newdiam);
            }
        }
    }
    fout &lt;&lt; setprecision(6) &lt;&lt; fixed &lt;&lt; res &lt;&lt; endl;
    return 0;
}
[/c]

标程：
[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;
#include &lt;math.h&gt;

#define INF    (1e40)

typedef struct Point Point;
struct Point {
    int x, y;
};

#define MAXPASTURE 150

int n;
double dist[MAXPASTURE][MAXPASTURE];
double diam[MAXPASTURE];
double fielddiam[MAXPASTURE];
Point pt[MAXPASTURE];
int field[MAXPASTURE];
int nfield;

double
ptdist(Point a, Point b)
{
    return sqrt((double)(b.x-a.x)*(b.x-a.x)+(double)(b.y-a.y)*(b.y-a.y));
}

/* mark the field containing pasture i with number m */
void
mark(int i, int m)
{
    int j;
    if(field[i] != -1) {
        assert(field[i] == m);
        return;
    }

    field[i] = m;
    for(j=0; j&lt;n; j++)
        if(dist[i][j] &lt; INF/2)
            mark(j, m);
}

void
main(void)
{
    FILE *fin, *fout;
    int i, j, k, c;
    double newdiam, d;

    fin = fopen(&quot;cowtour.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;cowtour.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    fscanf(fin, &quot;%d\n&quot;, &amp;n);
    for(i=0; i&lt;n; i++)
        fscanf(fin, &quot;%d %d\n&quot;, &amp;pt[i].x, &amp;pt[i].y);

    for(i=0; i&lt;n; i++) {
        for(j=0; j&lt;n; j++) {
            c = getc(fin);
            if(i == j)
                dist[i][j] = 0;
            else if(c == '0')
                dist[i][j] = INF;        /* a lot */
            else
                dist[i][j] = ptdist(pt[i], pt[j]);
        }
        assert(getc(fin) == '\n');
    }

    /* Floyd-Warshall all pairs shortest paths */
    for(k=0; k&lt;n; k++)
    for(i=0; i&lt;n; i++)
    for(j=0; j&lt;n; j++)
        if(dist[i][k]+dist[k][j] &lt; dist[i][j])
            dist[i][j] = dist[i][k]+dist[k][j];

    /* mark fields */
    for(i=0; i&lt;n; i++)
        field[i] = -1;
    for(i=0; i&lt;n; i++)
        if(field[i] == -1)
            mark(i, nfield++);

    /* find worst diameters involving pasture i, and for whole field */
    for(i=0; i&lt;n; i++) {
        for(j=0; j&lt;n; j++)
            if(diam[i] &lt; dist[i][j] &amp;&amp; dist[i][j] &lt; INF/2)
                diam[i] = dist[i][j];
        if(diam[i] &gt; fielddiam[field[i]])
            fielddiam[field[i]] = diam[i];
    }

    /* consider a new path between i and j */
    newdiam = INF;
    for(i=0; i&lt;n; i++)
    for(j=0; j&lt;n; j++) {
        if(field[i] == field[j])
            continue;

        d = diam[i]+diam[j]+ptdist(pt[i], pt[j]);
        if(d &lt; fielddiam[field[i]])
            d = fielddiam[field[i]];
        if(d &lt; fielddiam[field[j]])
            d = fielddiam[field[j]];

        if(d &lt; newdiam)
            newdiam = d;
    }

    fprintf(fout, &quot;%.6lf\n&quot;, newdiam);
    exit(0);
}
[/c]
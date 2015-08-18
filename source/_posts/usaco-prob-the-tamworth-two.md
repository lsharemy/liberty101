title: USACO - PROB The Tamworth Two
tags:
  - USACO
id: 1780
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-21 15:55:15
---

模拟题，模拟的步数开小了，竟然给过了

代码：<!--more-->
[c]
/*
ID: lmy0525
PROG: ttwo
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

string grid[10];
int fr, fc, cr, cc;
int d[4][2] = { {-1, 0}, {0, 1}, {1, 0}, {0, -1} }; // N E S W
int df, dc;

int main()
{
    fout.open(&quot;ttwo.out&quot;);
    fin.open(&quot;ttwo.in&quot;);
    for (int i = 0; i &lt; 10; i++)
    {
        fin &gt;&gt; grid[i];
    }
    for (int i = 0; i &lt; 10; i++)
    {
        for (int j = 0; j &lt; 10; j++)
        {
            if (grid[i][j] == 'F')
            {
                fr = i;
                fc = j;
            }
            if (grid[i][j] == 'C')
            {
                cr = i;
                cc = j;
            }
        }
    }

    bool flag = false;
    int min;
    for (int i = 1; i &lt;= 1000; i++)
    {
        // Farmer
        int nextfr = fr + d[df][0];
        int nextfc = fc + d[df][1];
        if (nextfr &lt; 0 || nextfr &gt; 9 || nextfc &lt; 0 || nextfc &gt; 9 || grid[nextfr][nextfc] == '*')
        {
            df = (df+1)%4;
        }
        else
        {
            fr = nextfr;
            fc = nextfc;
        }
        // Cows
        int nextcr = cr + d[dc][0];
        int nextcc = cc + d[dc][1];
        if (nextcr &lt; 0 || nextcr &gt; 9 || nextcc &lt; 0 || nextcc &gt; 9 || grid[nextcr][nextcc] == '*')
        {
            dc = (dc+1)%4;
        }
        else
        {
            cr = nextcr;
            cc = nextcc;
        }
        // meet?
        if (fr == cr &amp;&amp; fc == cc)
        {
            flag = true;
            min = i;
            break;
        }
    }
    if (flag) fout &lt;&lt; min &lt;&lt; endl;
    else fout &lt;&lt; 0 &lt;&lt; endl;
    return 0;
}
[/c]

标程：
[c]
/*
PROG: ttwo
ID: rsc001
*/

#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

char grid[10][10];

/* delta x, delta y position for moving north, east, south, west */
int deltax[] = { 0, 1, 0, -1 };
int deltay[] = { -1, 0, 1, 0 };

void
move(int *x, int *y, int *dir)
{
	int nx, ny;

	nx = *x+deltax[*dir];
	ny = *y+deltay[*dir];

	if(nx &lt; 0 || nx &gt;= 10 || ny &lt; 0 || ny &gt;= 10 || grid[ny][nx] == '*')
		*dir = (*dir + 1) % 4;
	else {
		*x = nx;
		*y = ny;
	}
}

void
main(void)
{
	FILE *fin, *fout;
	char buf[100];
	int i, x, y;
	int cowx, cowy, johnx, johny, cowdir, johndir;

	fin = fopen(&quot;ttwo.in&quot;, &quot;r&quot;);
	fout = fopen(&quot;ttwo.out&quot;, &quot;w&quot;);
	assert(fin != NULL &amp;&amp; fout != NULL);

	cowx = cowy = johnx = johny = -1;

	for(y=0; y&lt;10; y++) {
		fgets(buf, sizeof buf, fin);
		for(x=0; x&lt;10; x++) {
			grid[y][x] = buf[x];
			if(buf[x] == 'C') {
				cowx = x;
				cowy = y;
				grid[y][x] = '.';
			}
			if(buf[x] == 'F') {
				johnx = x;
				johny = y;
				grid[y][x] = '.';
			}
		}
	}

	assert(cowx &gt;= 0 &amp;&amp; cowy &gt;= 0 &amp;&amp; johnx &gt;= 0 &amp;&amp; johny &gt;= 0);

	cowdir = johndir = 0;	/* north */

	for(i=0; i&lt;160000 &amp;&amp; (cowx != johnx || cowy != johny); i++) {
		move(&amp;cowx, &amp;cowy, &amp;cowdir);
		move(&amp;johnx, &amp;johny, &amp;johndir);
	}

	if(cowx == johnx &amp;&amp; cowy == johny)
		fprintf(fout, &quot;%d\n&quot;, i);
	else
		fprintf(fout, &quot;0\n&quot;);
	exit(0);
}
[/c]
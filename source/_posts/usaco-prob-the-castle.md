title: USACO - PROB The Castle
tags:
  - USACO
id: 1728
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-18 13:33:05
---

我的：<!--more-->

[c]
/*
ID: lmy0525
PROG: castle
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
#include &lt;cmath&gt;

using namespace std;

ofstream fout;
ifstream fin;

int M, N;
int color[51][51];
int castle[51][51];
int sizes[51];

int dfs(int r, int c, int cur)
{
    int sum = 1;
    color[r][c] = cur;

    if ((castle[r][c] &amp; 1) == 0 &amp;&amp; c &gt; 1 &amp;&amp; color[r][c-1] == 0) sum += dfs(r, c-1, cur); // W
    if ((castle[r][c] &amp; 2) == 0 &amp;&amp; r &gt; 1 &amp;&amp; color[r-1][c] == 0) sum += dfs(r-1, c, cur); // N
    if ((castle[r][c] &amp; 4) == 0 &amp;&amp; c &lt; M &amp;&amp; color[r][c+1] == 0) sum += dfs(r, c+1, cur); // E
    if ((castle[r][c] &amp; 8 ) == 0 &amp;&amp; r &lt; N &amp;&amp; color[r+1][c] == 0) sum += dfs(r+1, c, cur); // S

    return sum;
}

int main() {
    fout.open(&quot;castle.out&quot;);
    fin.open(&quot;castle.in&quot;);
    fin &gt;&gt; M &gt;&gt; N;
    for (int i = 1; i &lt;= N; i++)
    {
        for (int j = 1; j &lt;= M; j++)
        {
            fin &gt;&gt; castle[i][j];
        }
    }

    int largest = 0;
    int cur_color = 1;
    for (int i = 1; i &lt;= N; i++)
    {
        for (int j = 1; j &lt;= M; j++)
        {
            if (color[i][j] == 0)
            {
                sizes[cur_color] = dfs(i, j, cur_color);
                largest = max(largest, sizes[cur_color]);
                cur_color++;
            }
        }
    }
    fout &lt;&lt; cur_color-1 &lt;&lt; endl;
    fout &lt;&lt; largest &lt;&lt; endl;
    int largest2 = 0;
    int wall_r;
    int wall_c;
    int wall_north;// 0 - east, 1 - north
    for (int j = 1; j &lt;= M; j++)
    {
        for (int i = N; i &gt;= 1; i--)
        {
            if ((castle[i][j] &amp; 2) != 0 &amp;&amp; i &gt; 1 &amp;&amp; color[i][j] != color[i-1][j]) 
            {
                int size = sizes[color[i][j]] + sizes[color[i-1][j]];
                if (size &gt; largest2)
                {
                    largest2 = size;
                    wall_r = i;
                    wall_c = j;
                    wall_north = 1;
                }
            }
            if ((castle[i][j] &amp; 4) != 0 &amp;&amp; j &lt; M &amp;&amp; color[i][j] != color[i][j+1])
            {
                int size = sizes[color[i][j]] + sizes[color[i][j+1]];
                if (size &gt; largest2)
                {
                    largest2 = size;
                    wall_r = i;
                    wall_c = j;
                    wall_north = 0;
                }
            }
        }
    }
    fout &lt;&lt; largest2 &lt;&lt; endl;
    fout &lt;&lt; wall_r &lt;&lt; &quot; &quot; &lt;&lt; wall_c &lt;&lt; &quot; &quot; &lt;&lt; (wall_north==1?'N':'E') &lt;&lt; endl;
    return 0;
}
[/c]

标程：
[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

#define MAXDIM 50
#define MAXN 100
#define MAXCOLOR 100
#define MAXROOM (MAXDIM*MAXDIM)

enum {
    Wwest = 1,
    Wnorth = 2,
    Weast = 4,
    Wsouth = 8
};

typedef struct Square	Square;
struct Square {
    int wall;
    int numbered;
    int room;
};

int wid, ht;
Square castle[MAXDIM][MAXDIM];
int roomsize[MAXROOM];

void
number(int room, int x, int y)
{
    int w;

    if(castle[x][y].numbered) {
	assert(castle[x][y].room == room);
	return;
    }

    castle[x][y].numbered = 1;
    castle[x][y].room = room;
    roomsize[room]++;

    w = castle[x][y].wall;

    if(x &gt; 0 &amp;&amp; !(w &amp; Wwest))
	number(room, x-1, y);

    if(x+1 &lt; wid &amp;&amp; !(w &amp; Weast))
	number(room, x+1, y);

    if(y &gt; 0 &amp;&amp; !(w &amp; Wnorth))
	number(room, x, y-1);

    if(y+1 &lt; ht &amp;&amp; !(w &amp; Wsouth))
	number(room, x, y+1);
}

void
main(void)
{
    FILE *fin, *fout;
    int x, y, w, nroom, bigroom;
    int i, n, m, mx, my;
    char mc;

    fin = fopen(&quot;castle.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;castle.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    fscanf(fin, &quot;%d %d&quot;, &amp;wid, &amp;ht);

    /* read in wall info */
    for(y=0; y&lt;ht; y++) {
	for(x=0; x&lt;wid; x++) {
	    fscanf(fin, &quot;%d&quot;, &amp;w);
	    castle[x][y].wall = w;
	}
    }

    /* number rooms */
    nroom = 0;
    for(y=0; y&lt;ht; y++)
    for(x=0; x&lt;wid; x++)
	if(!castle[x][y].numbered)
	    number(nroom++, x, y);

    /* find biggest room */
    bigroom = roomsize[0];
    for(i=1; i&lt;nroom; i++)
	if(bigroom &lt; roomsize[i])
	    bigroom = roomsize[i];

    /* look at best that can come of removing an east or north wall */
    m = 0;
    for(x=0; x&lt;wid; x++)
    for(y=ht-1; y&gt;=0; y--) {
	if(y &gt; 0 &amp;&amp; castle[x][y].room != castle[x][y-1].room) {
	    n = roomsize[castle[x][y].room] + roomsize[castle[x][y-1].room];
	    if(n &gt; m) {
		m = n;
		mx = x;
		my = y;
		mc = 'N';
	    }
	}
	if(x+1 &lt; wid &amp;&amp; castle[x][y].room != castle[x+1][y].room) {
	    n = roomsize[castle[x][y].room] + roomsize[castle[x+1][y].room];
	    if(n &gt; m) {
		m = n;
		mx = x;
		my = y;
		mc = 'E';
	    }
	}
    }

    fprintf(fout, &quot;%d\n&quot;, nroom);
    fprintf(fout, &quot;%d\n&quot;, bigroom);
    fprintf(fout, &quot;%d\n&quot;, m);
    fprintf(fout, &quot;%d %d %c\n&quot;, my+1, mx+1, mc);
    exit(0);
}
[/c]
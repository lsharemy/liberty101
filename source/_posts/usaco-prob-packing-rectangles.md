title: USACO - PROB Packing Rectangles
tags:
  - USACO
id: 1678
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-14 19:36:09
---

我的：<!--more-->

[c]
/*
ID: packrec
PROG: test
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;set&gt;

using namespace std;

struct Rect
{
    int w;
    int h;
}rect[4];

set &lt;int&gt; bestRects;
int best = 0;

void record(Rect r)
{
    if (r.w*r.h &lt; best || best == 0)
    {
        best = r.w*r.h;
        bestRects.clear();
    }
    if (r.w*r.h == best) bestRects.insert(min(r.w, r.h));
}

void check(Rect* r)
{
    Rect out;

    // 1
    out.w = 0;
    out.h = 0;
    for (int i = 0; i &lt; 4; i++)
    {
        out.w += r[i].w;
        out.h = max(out.h, r[i].h);
    }
    record(out);

    // 2
    out.w = 0;
    out.h = 0;
    for (int i = 0; i &lt; 3; i++)
    {
        out.w += r[i].w;
        out.h = max(out.h, r[i].h);
    }
    out.h += r[3].h;
    out.w = max(out.w, r[3].w);
    record(out);

    // 3
    out.w = r[0].w + r[1].w;
    out.h = max(r[0].h, r[1].h);
    out.h += r[2].h;
    out.w = max(out.w, r[2].w);
    out.w += r[3].w;
    out.h = max(out.h, r[3].h);
    record(out);

    // 4 &amp; 5
    out.w = r[0].w + r[1].w;
    out.h = max(r[0].h, r[1].h);
    out.w += max(r[2].w, r[3].w);
    out.h = max(out.h, r[2].h+r[3].h);
    record(out);

    // 6
    out.h = max(r[0].h+r[2].h, r[1].h+r[3].h);
    out.w = r[0].w+r[1].w;

    if (r[0].h &lt; r[1].h)
        out.w = max(out.w, r[2].w+r[1].w);
    if (r[0].h+r[2].h &gt; r[1].h)
        out.w = max(out.w, r[2].w+r[3].w);
    if (r[0].h &gt; r[1].h)
        out.w = max(out.w, r[0].w+r[3].w);

    out.w = max(out.w, r[3].w);
    out.w = max(out.w, r[2].w);
    record(out);
}

Rect rotate(Rect r)
{
    Rect temp;
    temp.w = r.h;
    temp.h = r.w;
    return temp;
}

void checkrotate(Rect* r, int n)
{
    if (n == 4)
    {
        check(r);
        return;
    }

    checkrotate(r, n+1);
    r[n] = rotate(r[n]);
    checkrotate(r, n+1);
    r[n] = rotate(r[n]);
}

void checkpermute(Rect* r, int n) // check all premutation
{
    if (n == 4)
    {
        checkrotate(r, 0);
        return;
    }

    for (int i = n; i &lt; 4; i++)
    {
        Rect t;
        t = r[n], r[n] = r[i], r[i] = t;
        checkpermute(r, n+1);
        t = r[n], r[n] = r[i], r[i] = t;
    }
}

int main() {
    ofstream fout (&quot;packrec.out&quot;);
    ifstream fin (&quot;packrec.in&quot;);
    for (int i = 0; i &lt; 4; i++)
    {
        fin &gt;&gt; rect[i].w &gt;&gt; rect[i].h;
    }
    checkpermute(rect, 0);
    fout &lt;&lt; best &lt;&lt; endl;
    for (set&lt; int &gt;::iterator it = bestRects.begin(); it != bestRects.end(); it++)
    {
        int temp = *it;
        fout &lt;&lt; temp &lt;&lt; &quot; &quot; &lt;&lt; best/temp &lt;&lt; endl;
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

typedef struct Rect Rect;
struct Rect {
    int wid;
    int ht;
};

Rect
rotate(Rect r)
{
    Rect nr;

    nr.wid = r.ht;
    nr.ht = r.wid;
    return nr;
}

int
max(int a, int b)
{
    return a &gt; b ? a : b;
}

int
min(int a, int b)
{
    return a &lt; b ? a : b;
}

int tot;
int bestarea;
int bestht[101];

void
record(Rect r)
{
    int i;

    if(r.wid*r.ht &lt; tot)
        *(long*)0=0;

    if(r.wid*r.ht &lt; bestarea || bestarea == 0) {
        bestarea = r.wid*r.ht;
        for(i=0; i&lt;=100; i++)
            bestht[i] = 0;
    }
    if(r.wid*r.ht == bestarea)
        bestht[min(r.wid, r.ht)] = 1;
}

void
check(Rect *r)
{
    Rect big;
    int i;

    /* schema 1: all lined up next to each other */
    big.wid = 0;
    big.ht = 0;
    for(i=0; i&lt;4; i++) {
        big.wid += r[i].wid;
        big.ht = max(big.ht, r[i].ht);
    }
    record(big);

    /* schema 2: first three lined up, fourth on bottom */
    big.wid = 0;
    big.ht = 0;
    for(i=0; i&lt;3; i++) {
        big.wid += r[i].wid;
        big.ht = max(big.ht, r[i].ht);
    }
    big.ht += r[3].ht;
    big.wid = max(big.wid, r[3].wid);
    record(big);

    /* schema 3: first two lined up, third under them, fourth to side */
    big.wid = r[0].wid + r[1].wid;
    big.ht = max(r[0].ht, r[1].ht);
    big.ht += r[2].ht;
    big.wid = max(big.wid, r[2].wid);
    big.wid += r[3].wid;
    big.ht = max(big.ht, r[3].ht);
    record(big);

    /* schema 4, 5: first two rectangles lined up, next two stacked */
    big.wid = r[0].wid + r[1].wid;
    big.ht = max(r[0].ht, r[1].ht);
    big.wid += max(r[2].wid, r[3].wid);
    big.ht = max(big.ht, r[2].ht+r[3].ht);
    record(big);

    /*
     * schema 6: first two pressed next to each other, next two on top, like:
     * 2 3
     * 0 1
     */
    big.ht = max(r[0].ht+r[2].ht, r[1].ht+r[3].ht);
    big.wid = r[0].wid + r[1].wid;

    /* do 2 and 1 touch? */
    if(r[0].ht &lt; r[1].ht)
        big.wid = max(big.wid, r[2].wid+r[1].wid);
    /* do 2 and 3 touch? */
    if(r[0].ht+r[2].ht &gt; r[1].ht)
        big.wid = max(big.wid, r[2].wid+r[3].wid);
    /* do 0 and 3 touch? */
    if(r[1].ht &lt; r[0].ht)
        big.wid = max(big.wid, r[0].wid+r[3].wid);

    /* maybe 2 or 3 sits by itself */
    big.wid = max(big.wid, r[2].wid);
    big.wid = max(big.wid, r[3].wid);
    record(big);
}

void
checkrotate(Rect *r, int n)
{
    if(n == 4) {
        check(r);
        return;
    }

    checkrotate(r, n+1);
    r[n] = rotate(r[n]);
    checkrotate(r, n+1);
    r[n] = rotate(r[n]);
}

void
checkpermute(Rect *r, int n)
{
    Rect t;
    int i;

    if(n == 4)
        checkrotate(r, 0);

    for(i=n; i&lt;4; i++) {
        t = r[n], r[n] = r[i], r[i] = t;    /* swap r[i], r[n] */
        checkpermute(r, n+1);
        t = r[n], r[n] = r[i], r[i] = t;    /* swap r[i], r[n] */
    }
}

void
main(void)
{
    FILE *fin, *fout;
    Rect r[4];
    int i;

    fin = fopen(&quot;packrec.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;packrec.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    for(i=0; i&lt;4; i++)
        fscanf(fin, &quot;%d %d&quot;, &amp;r[i].wid, &amp;r[i].ht);

    tot=(r[0].wid*r[0].ht+r[1].wid*r[1].ht+r[2].wid*r[2].ht+r[3].wid*r[3].ht);

    checkpermute(r, 0);
    fprintf(fout, &quot;%d\n&quot;, bestarea);
    for(i=0; i&lt;=100; i++)
        if(bestht[i])
            fprintf(fout, &quot;%d %d\n&quot;, i, bestarea/i);
    exit(0);
}

[/c]

好吧，又偷瞄了标程，不过学到了很多东西，这个题考察的应该是DFS，一个是DFS产生24种排列，还有就是DFS产生4个矩形旋转产生16种可能的结果，然后6种放置方法就要一种一种考虑了。代码少敲了一个+号，于是又学vimgdb学了好久。
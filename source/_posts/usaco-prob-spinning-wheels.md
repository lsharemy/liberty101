title: USACO - PROB Spinning Wheels
tags:
  - USACO
id: 1817
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-23 22:15:43
---

模拟

我的：<!--more-->

[c]
/*
ID: lmy0525
PROG: spin
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

int speed[5];
vector &lt; pair&lt;int, int&gt; &gt; wedge[5];
int mark[360];

int main()
{
    fout.open(&quot;spin.out&quot;);
    fin.open(&quot;spin.in&quot;);
    for (int i = 0; i &lt; 5; i++)
    {
        fin &gt;&gt; speed[i];
        int nwedge;
        fin &gt;&gt; nwedge;
        for (int j = 0; j &lt; nwedge; j++)
        {
            int start, extent;
            fin &gt;&gt; start &gt;&gt; extent;
            wedge[i].push_back(make_pair(start, extent));
        }
    }

    int i;
    bool flag = false;
    for (i = 0; i &lt; 360; i++)
    {
        memset(mark, 0, sizeof(mark));
        for (int j = 0; j &lt; 5; j++)
        {
            int pos = (speed[j]*i)%360; // pos of the wheel
            for (int w = 0; w &lt; wedge[j].size(); w++) // wedge
            {
                int startpos = wedge[j][w].first;
                int extent = wedge[j][w].second;
                for ( int idx = startpos; idx &lt;= startpos+extent; idx++)
                {
                    mark[(pos+idx)%360]++;
                }
            }
        }
        for (int j = 0; j &lt; 360; j++)
        {
            if (mark[j] == 5)
            {
                flag = true;
                break;
            }
        }
        if (flag) break;
    }
    if (flag) fout &lt;&lt; i &lt;&lt; endl;
    else fout &lt;&lt; &quot;none&quot; &lt;&lt; endl;
    return 0;
}
[/c]
标程：
[c]
#include &lt;stdio.h&gt;
#include &lt;assert.h&gt;
#include &lt;string.h&gt;

int speed[5];      /* speed of each wheel */
int wedgest[5][5]; /* start of each wedge (-1 == no wedge) */
int wedglen[5][5]; /* length of each wedge */

int pos[5];        /* angular position of each wheel */
int t;             /* time (in seconds) since start */

/* (light[deg] &gt;&gt; wid) &amp; 0x1 is true if and only if there
   is a wedge in wheel wid that a light can shine through at
   angle deg */
int light[360];    

/* mark all the degrees we can see through wheel w */
void mark_light(int w)
 {
  int lv, lv2; /* loop variables */
  int wpos; /* wedge position */

  for (lv = 0; lv &lt; 5; lv++)
   {
    if (wedglen[w][lv] &lt; 0) /* no more wedges for this wheel */
      break;

    /* start of wedge */
    wpos = (pos[w] + wedgest[w][lv]) % 360;

    for (lv2 = 0; lv2 &lt;= wedglen[w][lv]; lv2++)
     { /* throughout extent of wedge */
      light[wpos] |= (1 &lt;&lt; w); /* mark as hole in wheel */
      wpos = (wpos + 1) % 360; /* go to the next degree */
     }
   }
 }

int main(int argc, char **argv)
 {
  FILE *fp;
  FILE *fout;
  int w, f;
  int lv, lv2;

  fp = fopen(&quot;spin.in&quot;, &quot;r&quot;);
  fout = fopen(&quot;spin.out&quot;, &quot;w&quot;);
  assert(fp);
  assert(fout);

  /* read in the data */
  for (lv = 0; lv &lt; 5; lv++)
   {
    fscanf (fp, &quot;%d %d&quot;, &amp;speed[lv], &amp;w);
    for (lv2 = 0; lv2 &lt; w; lv2++)
      fscanf (fp, &quot;%d %d&quot;, &amp;wedgest[lv][lv2], &amp;wedglen[lv][lv2]);

    /* mark the rest of the wedges as not existing for this wheel */
    for (; lv2 &lt; 5; lv2++)
      wedglen[lv][lv2] = -1;
   }

  f = 0;
  while (t &lt; 360) /* for each time step */
   {
    memset(light, 0, sizeof(light));

    /* mark the degrees we can see through each wheel */
    for (lv = 0; lv &lt; 5; lv++)
      mark_light(lv);

    for (lv = 0; lv &lt; 360; lv++)
      if (light[lv] == 31) /* we can shine a light through all five wheels */
        f = 1;

    if (f) break; /* we found a match! */

    /* make a time step */
    t++;
    for (lv = 0; lv &lt; 5; lv++)
      pos[lv] = (pos[lv] + speed[lv]) % 360;
   }

  /* after 360 time steps, all the wheels have returned to their
     original location */
  if (t &gt;= 360) fprintf (fout, &quot;none\n&quot;);
  else fprintf (fout, &quot;%i\n&quot;, t);

  return 0;
 }
[/c]
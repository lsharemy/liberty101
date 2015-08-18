title: USACO - PROB Feed Ratios
tags:
  - USACO
id: 1820
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-24 13:27:00
---

用的暴力，用解方程也可以，但是遵循用最Stupid方法的原则。

代码：<!--more-->
[c]
/*
ID: lmy0525
PROG: ratios
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

int goal[3];
int mixture[3][3];

int main()
{
    fout.open(&quot;ratios.out&quot;);
    fin.open(&quot;ratios.in&quot;);

    for (int i = 0; i &lt; 3; i++) fin &gt;&gt; goal[i];
    for (int i = 0; i &lt; 3; i++)
    for (int j = 0; j &lt; 3; j++)
        fin &gt;&gt; mixture[i][j];
    if (goal[0] == 0 &amp;&amp; goal[1] == 0 &amp;&amp; goal[2] == 0)
    {
        fout &lt;&lt; &quot;0 0 0 0&quot; &lt;&lt; endl;
        return 0;
    }
    int i[3];
    int minsum = 30000;
    int threeratio[3];
    int goalratio;
    bool flag = false;
    for (i[0] = 0; i[0] &lt; 100; i[0]++)
    for (i[1] = 0; i[1] &lt; 100; i[1]++)
    for (i[2] = 0; i[2] &lt; 100; i[2]++)
    {
        int temp[3] = {0, 0, 0};
        for (int j = 0; j &lt; 3; j++) // three components
        {
            for (int k = 0; k &lt; 3; k++) // three mixture
            {
                temp[j] += mixture[k][j]*i[k];
            }
        }
        int sum = 0;
        for (int j = 0; j &lt; 3; j++) sum += temp[j];
        if (sum == 0) continue;
        int goalsum = 0;
        for (int j = 0; j &lt; 3; j++) goalsum += goal[j];
        if ( sum % goalsum != 0) continue;
        int ratio = sum / goalsum;
        if (ratio &gt;= 100) continue;
        if (ratio*goal[0] == temp[0] &amp;&amp; ratio*goal[1] == temp[1] &amp;&amp; ratio*goal[2] == temp[2])
        {
            // cool combine
            if (sum &lt; minsum)
            {
                minsum = sum;
                flag = true;
                for (int k = 0; k &lt; 3; k++) threeratio[k] = i[k];
                goalratio = ratio;
            }
        }
    }
    if (flag)
    {
        for (int i = 0; i &lt; 3; i++) fout &lt;&lt; threeratio[i] &lt;&lt; &quot; &quot;;
        fout &lt;&lt; goalratio &lt;&lt; endl;
    }
    else
    {
        fout &lt;&lt; &quot;NONE&quot; &lt;&lt; endl;
    }
    return 0;
}
[/c]

标程：
[c]
#include &lt;stdio.h&gt;

/* the goal ratio */
int goal[3];

/* the ratio of the feeds */
int ratios[3][3];

/* the best solution found so far */
int min;
int mloc[4]; /* amounts of ratio 1, 2, and 3, and the amount of ratio 4 prod */

/* the amount of each type of component in the feed */
int sum[3];

int main(int argc, char **argv)
 {
  FILE *fout, *fin;
  int lv, lv2, lv3; /* loop variables */
  int t, s; /* temporary variables */
  int gsum; /* the sum of the amounts of components in the goal mixture */

  if ((fin = fopen(&quot;ratios.in&quot;, &quot;r&quot;)) == NULL)
   {
    perror (&quot;fopen fin&quot;);
    exit(1);
   }
  if ((fout = fopen(&quot;ratios.out&quot;, &quot;w&quot;)) == NULL)
   {
    perror (&quot;fopen fout&quot;);
    exit(1);
   }

  /* read in data */
  fscanf (fin, &quot;%d %d %d&quot;, &amp;goal[0], &amp;goal[1], &amp;goal[2]);
  for (lv = 0; lv &lt; 3; lv++)
    fscanf (fin, &quot;%d %d %d&quot;, ratios[lv]+0, ratios[lv]+1, ratios[lv]+2);

  gsum = goal[0] + goal[1] + goal[2];

  min = 301; /* worst than possible = infinity */

  /* boundary case (this ensures gsum != 0) */
  if (gsum == 0)
   {
    fprintf (fout, &quot;0 0 0 0\n&quot;);
    return 0;
   }

  for (lv = 0; lv &lt; 100; lv++)
    for (lv2 = 0; lv2 &lt; 100; lv2++)
     { /* for each amout of the first two types of mixtures */
      sum[0] = lv*ratios[0][0] + lv2*ratios[1][0];
      sum[1] = lv*ratios[0][1] + lv2*ratios[1][1];
      sum[2] = lv*ratios[0][2] + lv2*ratios[1][2];

      if (lv + lv2 &gt; min) break;

      for (lv3 = 0; lv3 &lt; 100; lv3++)
       {
        s = lv + lv2 + lv3;
	if (s &gt;= min) break; /* worse than we've found already */

        /* calculate a guess at the multiples of the goal we've obtained */
	/* use gsum so we don't have to worry about divide by zero */
        t = (sum[0] + sum[1] + sum[2]) / gsum;
	if (t != 0 &amp;&amp; sum[0] == t*goal[0] &amp;&amp; 
	        sum[1] == t*goal[1] &amp;&amp; sum[2] == t*goal[2])
	 { /* we've found a better solution! */
	  /* update min */
	  min = s;

	  /* store the solution */
	  mloc[0] = lv;
	  mloc[1] = lv2;
	  mloc[2] = lv3;
	  mloc[3] = t;
	 }

        /* add another 'bucket' of feed #2 */
        sum[0] += ratios[2][0];
        sum[1] += ratios[2][1];
        sum[2] += ratios[2][2];
       }
     }
  if (min == 301) fprintf (fout, &quot;NONE\n&quot;); /* no solution found */
  else fprintf (fout, &quot;%d %d %d %d\n&quot;, mloc[0], mloc[1], mloc[2], mloc[3]);
  return 0;
 }
[/c]

标程2：
[c]
// 木有代码，但是解释了如何用矩阵的知识来解
[/c]
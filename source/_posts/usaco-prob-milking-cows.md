title: "USACO - \tPROB Milking Cows"
tags:
  - USACO
id: 1615
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-12 16:55:39
---

终于用vim敲完了一个程序

我的：<!--more-->

[c]
/*
ID: lmy0525
PROG: milk2
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;vector&gt;
#include &lt;algorithm&gt;

using namespace std;

vector &lt; pair&lt;int, int&gt; &gt; times;

int main()
{
    ofstream fout (&quot;milk2.out&quot;);
    ifstream fin (&quot;milk2.in&quot;);
    int N; // 1 - 5000
    fin &gt;&gt; N;
    for (int i = 0; i &lt; N; i++)
    {
         int left, right;
	 fin &gt;&gt; left &gt;&gt; right;
	 times.push_back(make_pair(left, 0));
	 times.push_back(make_pair(right, 1));
    }
    sort(times.begin(), times.end());
    int t1 = 0, t2 = 0;
    int start = times[0].first;
    int cnt = 0;
    for (int i = 0; i &lt; times.size(); i++)
    {
        int t = times[i].first;
	int flag = times[i].second;
	if (flag == 0)
	{
	    if (cnt == 0)
	    {
		t1 = max(t1, t-start);
	        start = t;
	    }
	    cnt++;
	}
	else if (flag == 1)
	{
	    cnt--;
	    if (cnt == 0)
	    {
	        t2 = max(t2, t-start);
		start = t;
	    }
	}
    }
    fout &lt;&lt; t2 &lt;&lt; &quot; &quot; &lt;&lt; t1 &lt;&lt; endl;
    return 0;
}
[/c]

标程：

[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

#define MAXMILKING 5000

typedef struct Milking	Milking;
struct Milking {
    int begin;
    int end;
};

Milking milking[MAXMILKING];
int nmilking;

int
milkcmp(const void *va, const void *vb)
{
    Milking *a, *b;

    a = (Milking*)va;
    b = (Milking*)vb;

    if(a-&gt;begin &gt; b-&gt;begin)
	return 1;
    if(a-&gt;begin &lt; b-&gt;begin)
	return -1;
    return 0;
}

void
main(void)
{
    FILE *fin, *fout;
    int i, j, t, tmilk, tnomilk;
    Milking cur;

    fin = fopen(&quot;milk2.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;milk2.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    /* read input, sort */
    fscanf(fin, &quot;%d&quot;, &amp;nmilking);
    for(i=0; i&lt;nmilking; i++)
	fscanf(fin, &quot;%d %d&quot;, &amp;milking[i].begin, &amp;milking[i].end);

    qsort(milking, nmilking, sizeof(Milking), milkcmp);

    /* walk over list, looking for long periods of time */
    /* tmilk = longest milking time */
    /* tnomilk = longest non-milking time */
    /* cur = current span of milking time being considered */

    tmilk = 0;
    tnomilk = 0;
    cur = milking[0];
    for(i=1; i&lt;nmilking; i++) {
	if(milking[i].begin &gt; cur.end) {	/* a down time */
	    t = milking[i].begin - cur.end;
	    if(t &gt; tnomilk)
		tnomilk = t;

	    t = cur.end - cur.begin;
	    if(t &gt; tmilk)
		tmilk = t;

	    cur = milking[i];
	} else {
	    if(milking[i].end &gt; cur.end)
		cur.end = milking[i].end;
	}
    }

    /* check final milking period */
    t = cur.end - cur.begin;
    if(t &gt; tmilk)
	tmilk = t;

    fprintf(fout, &quot;%d %d\n&quot;, tmilk, tnomilk);
    exit(0);
}
[/c]

标程是不断合并重叠的时间，直到碰到一个间隙，然后计算一个non-milking时间和milking时间，得到最大值。

另外一种解法：

[c]
/* sort the starting and ending times, then go through them from
 start to finish, keeping track of how many farmers are milking
 between each &quot;event&quot; (a single farmer starting and stopping). */

#include &lt;fstream.h&gt;
#include &lt;stdlib.h&gt;

struct event
{
 long seconds;   /* seconds since 5 am */
 signed char ss; /* start = 1, stop = -1 (delta number of farmers milking)
*/
};

int eventcmp (const event *a, const event *b)
{
 if (a-&gt;seconds != b-&gt;seconds)
  return (a-&gt;seconds - b-&gt;seconds); /* 300 before 500 */

 return (b-&gt;ss - a-&gt;ss); /* 1 (start) before -1 (stop) */
}

int main ()
{
 ifstream in;
 ofstream out;

 in.open(&quot;milk2.in&quot;);
 out.open(&quot;milk2.out&quot;);

 int num_intervals, num_events, i;
 event events[5000 * 2];

 in &gt;&gt; num_intervals;
 num_events = num_intervals * 2;
 for (i = 0; i &lt; num_intervals; ++i)
 {
  in &gt;&gt; events[2*i  ].seconds; events[2*i  ].ss = 1;
  in &gt;&gt; events[2*i+1].seconds; events[2*i+1].ss = -1;
 }

 qsort(events, num_events, sizeof(event),
  (int(*)(const void*, const void*)) eventcmp);

/* for (i = 0; i &lt; num_events; ++i)
  out &lt;&lt; events[i].seconds
    &lt;&lt; (events[i].ss == 1 ? &quot; start&quot; : &quot; stop&quot;) &lt;&lt; endl; */

 int num_milkers = 0, was_none = 1;
 int longest_nomilk = 0, longest_milk = 0;
 int istart, ilength;

 for (i = 0; i &lt; num_events; ++i)
 {
  num_milkers += events[i].ss;

  if (!num_milkers &amp;&amp; !was_none)
  {
   /* there are suddenly no milkers. */
   ilength = (events[i].seconds - istart);
   if (ilength &gt; longest_milk)
    longest_milk = ilength;
   istart = events[i].seconds;
  }
  else if (num_milkers &amp;&amp; was_none)
  {
   /* there are suddenly milkers. */
   if (i != 0)
   {
    ilength = (events[i].seconds - istart);
    if (ilength &gt; longest_nomilk)
     longest_nomilk = ilength;
   }
   istart = events[i].seconds;
  }

  was_none = (num_milkers == 0);
 }

 out &lt;&lt; longest_milk &lt;&lt; &quot; &quot; &lt;&lt; longest_nomilk &lt;&lt; endl;

 return 0;
}
[/c]

这个解法和我的比较相似，把开始时间和结束时间表示成不同的事件，然后遍历这个事件的list，并计算得到时间。
title: USACO - PROB Healthy Holsteins
tags:
  - USACO
id: 1739
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-19 11:10:30
---

这道题有话说，忽忽。虽然题目的考察是穷举所有集合而已，但是要求的是一个最小（元素个数最少）的符合要求的集合，所以我就想，怎么样能按集合元素递增的顺序枚举集合呢？我想到了之前在[演算法筆記](http://www.csie.ntnu.edu.tw/~u91029/Backtracking.html#a4)看到的枚举相关的内容，结果没有发现（倒是发现了该页程序中的一个小bug，然后版主的[留言板](http://algonote.wordpress.com/)反馈了，好吧，我又开小差了）
-----------------
太晚了就回去睡觉了
----------
早上起来，继续想这个问题，于是去搜了一下（关键字“子集 枚举 顺序”），看到了[这篇文章](http://blog.csdn.net/zcsylj/article/details/6629715)，虽然文章排版有点问题，不过我没有瞬间关掉，因为发现了很有条理，举了3种枚举方法，最后一种就是我想要的，而且还链了[另外一个博客](http://www.thelowlyprogrammer.com/2010/04/indexing-and-enumerating-subsets-of.html)，我也链了过去，看了前面一段，又被其中的一个叫做“[nerd sniped](http://xkcd.com/356/)”的链接吸引了，再想这是神马意思，于是又链过去了，发现是个神站（对我来说，画画能画很好的都是神）诶，瞬间被萌到了，果断收藏，顺便把前面那个“[另外一个博客](http://www.thelowlyprogrammer.com/2010/04/indexing-and-enumerating-subsets-of.html)”也收藏了，目测作者也是个神。

还发现了[一篇论文](http://www.applied-math.org/subset.pdf)，这篇论文在之前那个博客里都提到了，去google scholar上搜索来看了下，竟然有代码。然后果断开始做题，好吧，我终于开始做题了，竟然敲出了一次bug free的代码，一次编译通过，一次AC，忽忽，我好开心～

贴代码了：<!--more-->

[c]
/*
ID: lmy0525
PROG: holstein
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

int V, G;
int require[25];
int scoops[15][25];
int solution[15];
bool done;

void generate(int pos, int s)
{
    if (done) return;
    if (pos &lt; s)
    {
        if (pos == 0)
        {
            for (int i = 0; i &lt; G; i++)
            {
                solution[pos] = i;
                generate(pos+1, s);
                solution[pos] = 0;
            }
        }
        else
        {
            for (int i = solution[pos-1]+1; i &lt; G; i++)
            {
                solution[pos] = i;
                generate(pos+1, s);
                solution[pos] = 0;
            }
        }
    }
    else
    {
        bool flag = true;
        for (int i = 0; i &lt; V; i++)
        {
            int temp = 0;
            for (int j = 0; j &lt; s; j++)
            {
                temp += scoops[solution[j]][i];
            }
            if ( temp &lt; require[i] )
            {
                flag = false;
                break;
            }
        }

        if (flag)
        {
            fout &lt;&lt; s;
            for (int i = 0; i &lt; s; i++)
            {
                fout &lt;&lt; &quot; &quot; &lt;&lt; solution[i]+1;
            }
            fout &lt;&lt; endl;
            done = true;
        }
    }
}

int main() {
    fout.open(&quot;holstein.out&quot;);
    fin.open(&quot;holstein.in&quot;);
    fin &gt;&gt; V;
    for (int i = 0; i &lt; V; i++)
    {
        fin &gt;&gt; require[i];
    }
    fin &gt;&gt; G;
    for (int i = 0; i &lt; G; i++)
    {
        for (int j = 0; j &lt; V; j++)
        {
            fin &gt;&gt; scoops[i][j];
        }
    }
    for (int i = 0; i &lt;= G; i++) // generate set with size i
    {
        generate(0, i);
    }
    return 0;
}
[/c]

标程是枚举所有的，而我的是按集合大小递增的顺序枚举的，直到找到一个合法的解，所以平均情况我的应该比标程快些吧

标程：
[c]
#include &lt;stdio.h&gt;
#include &lt;assert.h&gt;

#define MAXV 25
#define MAXF 15

int req[MAXV]; /* the vitamin requirements */
int numv; /* number of vitamins */

int feeds[MAXF][MAXV]; /* the vitamin within each feed */
int numf; /* number of feeds */

int best; /* the minimum number of feeds to use found thus far */
int bestf[MAXF]; /* the set */

int curf[MAXF]; /* the current set of feeds being considered */

void find_feed(int fcnt, int fid)
 { /* fcnt is the number of feeds in the current mixture,
      fid is the identifier of the first feed to try adding (last feed + 1) */
  int lv;

  /* check if the requirement has been met */
  for (lv = 0; lv &lt; numv; lv++)
    if (req[lv] &gt; 0) break; 
  if (lv &gt;= numv)
   { /* all the requirements are met */
    /* we know this is better, since we wouldn't have checked it otherwise
       (see below) */
    best = fcnt;
    for (lv = 0; lv &lt; best; lv++)
      bestf[lv] = curf[lv];
    return;
   }

  while (fid &lt; numf &amp;&amp; fcnt+1 &lt; best)
   { /* try adding each feed to the mixture */
     /* the fcnt+1 &lt; best ensures that we stop if there's no hope
	in finding a better solution than one found already */

    /* add the vitamins from this feed */
    for (lv = 0; lv &lt; numv; lv++)
      req[lv] -= feeds[fid][lv]; 
    curf[fcnt] = fid; /* put it in the list */

    find_feed(fcnt+1, fid+1); 

    /* undo adding the vitamins */
    for (lv = 0; lv &lt; numv; lv++)
      req[lv] += feeds[fid][lv];

    /* next feed */
    fid++;
   }
 }

int main(void) 
 {
  FILE *fin, *fout;
  int lv, lv2;

  fin = fopen(&quot;holstein.in&quot;, &quot;r&quot;);
  fout = fopen(&quot;holstein.out&quot;, &quot;w&quot;);
  assert(fin);
  assert(fout);

  fscanf (fin, &quot;%d&quot;, &amp;numv);
  for (lv = 0; lv &lt; numv; lv++)
    fscanf (fin, &quot;%d&quot;, &amp;req[lv]);
  fscanf (fin, &quot;%d&quot;, &amp;numf);
  for (lv = 0; lv &lt; numf; lv++)
    for (lv2 = 0; lv2 &lt; numv; lv2++)
      fscanf (fin, &quot;%d&quot;, &amp;feeds[lv][lv2]);

  best = numf+1;
  find_feed(0, 0);

  fprintf (fout, &quot;%i&quot;, best);
  for (lv = 0; lv &lt; best; lv++) 
    fprintf (fout, &quot; %i&quot;, bestf[lv]+1);
  fprintf (fout, &quot;\n&quot;);
  return 0;
 }
[/c]
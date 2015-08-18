title: USACO - PROB Hamming Codes
tags:
  - USACO
id: 1743
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-19 12:51:01
---

我的：<!--more-->
[c]
/*
ID: lmy0525
PROG: hamming
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
#include &lt;cmath&gt;
#include &lt;vector&gt;

using namespace std;

ofstream fout;
ifstream fin;

int N, B, D;
vector &lt;int&gt; res;

bool check (int n)
{
    for (int i = 0; i &lt; res.size(); i++)
    {
        if (__builtin_popcount(res[i] ^ n) &lt; D)
        {
            return false;
        }
    }
    return true;
}

int main() {
    fout.open(&quot;hamming.out&quot;);
    fin.open(&quot;hamming.in&quot;);
    fin &gt;&gt; N &gt;&gt; B &gt;&gt; D;
    res.push_back(0);
    for (int i = 1; i &lt; 1&lt;&lt;B; i++)
    {
        if (res.size() == N) break;
        if (check(i))
        {
            res.push_back(i);
        }
    }
    int i;
    for (i = 0; i &lt; res.size(); i++)
    {
        if (i % 10 != 0) fout &lt;&lt; &quot; &quot;;
        fout &lt;&lt; res[i];
        if (i % 10 == 9) fout &lt;&lt; endl;
    }
    if (i % 10 != 0) fout &lt;&lt; endl;
    return 0;
}
[/c]

标程：
[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;iostream.h&gt;
#define MAX (1 &lt;&lt; 8 + 1)
#define NMAX 65
#define BMAX 10
#define DMAX 10

int nums[NMAX], dist[MAX][MAX];
int N, B, D, maxval;
FILE *in, *out;

void findgroups(int cur, int start) {
    int a, b, canuse;
    char ch;
    if (cur == N) {
        for (a = 0; a &lt; cur; a++) {
            if (a % 10)
                fprintf(out, &quot; &quot;);
            fprintf(out, &quot;%d&quot;, nums[a]);
            if (a % 10 == 9 || a == cur-1)
                fprintf(out, &quot;\n&quot;);
        }
        exit(0);
    }
    for (a = start; a &lt; maxval; a++) {
        canuse = 1;
        for (b = 0; b &lt; cur; b++)
            if (dist[nums[b]][a] &lt; D) {
                canuse = 0;
                break;
            }
        if (canuse) {
            nums[cur] = a;
            findgroups(cur+1, a+1);
        }
    }
}

int main() {
    in = fopen(&quot;hamming.in&quot;, &quot;r&quot;);
    out = fopen(&quot;hamming.out&quot;, &quot;w&quot;);
    int a, b, c;
    fscanf(in, &quot;%d%d%d&quot;, &amp;N, &amp;B, &amp;D);
    maxval = (1 &lt;&lt; B);
    for (a = 0; a &lt; maxval; a++)
        for (b = 0; b &lt; maxval; b++) {
            dist[a][b] = 0;
            for (c = 0; c &lt; B; c++) 
                if (((1 &lt;&lt; c) &amp; a) != ((1 &lt;&lt; c) &amp; b))
                    dist[a][b]++;
        }
    nums[0] = 0;
    findgroups(1, 1);
    return 0;
}
[/c]
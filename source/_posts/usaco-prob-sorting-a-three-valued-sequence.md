title: USACO - PROB Sorting A Three-Valued Sequence
tags:
  - USACO
id: 1733
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-18 16:11:52
---

我的：<!--more-->

[c]
/*
ID: lmy0525
PROG: sort3
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

int records[1000];
int cnt[3];
int partcnt[3][3];

int swapfunc(int part1, int value1, int part2, int value2)
{
    int res = min(partcnt[part1][value1], partcnt[part2][value2]);
    partcnt[part1][value1] -= res;
    partcnt[part1][value2] += res;
    partcnt[part2][value2] -= res;
    partcnt[part2][value1] += res;
    return res;
}

int main() {
    fout.open(&quot;sort3.out&quot;);
    fin.open(&quot;sort3.in&quot;);
    int N;
    fin &gt;&gt; N;
    for (int i = 0; i &lt; N; i++)
    {
        fin &gt;&gt; records[i];
        cnt[records[i]-1]++;
    }
    int i = 0;
    for (; i &lt; cnt[0]; i++)
    {
        if (records[i] == 1) partcnt[0][0]++;
        else if (records[i] == 2) partcnt[0][1]++;
        else partcnt[0][2]++;
    }
    for (; i &lt; cnt[0]+cnt[1]; i++)
    {
        if (records[i] == 1) partcnt[1][0]++;
        else if (records[i] == 2) partcnt[1][1]++;
        else partcnt[1][2]++;
    }
    for (; i &lt; cnt[0]+cnt[1]+cnt[2]; i++)
    {
        if (records[i] == 1) partcnt[2][0]++;
        else if (records[i] == 2) partcnt[2][1]++;
        else partcnt[2][2]++;
    }
    int swapcnt = 0;

    swapcnt += swapfunc (0, 1, 1, 0);
    swapcnt += swapfunc (0, 2, 2, 0);
    swapcnt += swapfunc (1, 2, 2, 1);
    swapcnt += 2 * (cnt[0] - partcnt[0][0]);
    fout &lt;&lt; swapcnt &lt;&lt; endl;
    return 0;
}
[/c]

之前在USACO – TEXT Greedy Algorithm中举的一个例子就是这个，也讲了怎么贪心了，所以思路都是一样的。标程1是完全的模拟，不过因为没有交换的必要，所以有了标程2和标程3，我的跟标程3比较像，不过写的还是冗余了点T_T。

标程1：
[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

#define MAXN 	1000

int n;
int have[MAXN];
int want[MAXN];

int
intcmp(const void *a, const void *b)
{
    return *(int*)a - *(int*)b;
}

void
main(void)
{
    int i, j, k, n, nn[4], nswap, nbad;
    FILE *fin, *fout;

    fin = fopen(&quot;sort3.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;sort3.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    fscanf(fin, &quot;%d&quot;, &amp;n);

    for(i=0; i&lt;n; i++) {
	fscanf(fin, &quot;%d&quot;, &amp;have[i]);
	want[i] = have[i];
    }
    qsort(want, n, sizeof(want[0]), intcmp);

    /* swaps that correct two elements */
    nswap = 0;
    for(i=0; i&lt;n; i++)
    for(j=0; j&lt;n; j++) {
	if(have[i] != want[i] &amp;&amp; have[j] != want[j]
	        &amp;&amp; have[i] == want[j] &amp;&amp; have[j] == want[i]) {
	    have[i] = want[i];
	    have[j] = want[j];
	    nswap++;
	}
    }

    /* remainder are pairs of swaps that correct three elements */
    nbad = 0;
    for(i=0; i&lt;n; i++)
	if(have[i] != want[i])
	    nbad++;

    assert(nbad%3 == 0);
    nswap += nbad/3 * 2;

    fprintf(fout, &quot;%d\n&quot;, nswap);
    exit(0);
}
[/c]

标程2：

[c]
#include &lt;stdio.h&gt;

int list[1000], N, res, count[3], start[3];
in[3][3]; // this counts the number of &quot;1&quot;s in bucket &quot;2&quot;, ...

void readFile() {
    FILE *f; int i;
    f = fopen(&quot;sort3.in&quot;, &quot;r&quot;);
    fscanf(f, &quot;%d&quot;, &amp;N);
    for(i = 0; i &lt; N; i++) {
        fscanf(f, &quot;%d&quot;, &amp;list[i]);
    }
    fclose(f);
}

void writeFile() {
    FILE *f;
    f = fopen(&quot;sort3.out&quot;, &quot;w&quot;);
    fprintf(f, &quot;%d\n&quot;, res);
    fclose(f);
}

void findBuckets() {
    int i;
    for(i = 0; i &lt; N; i++) count[list[i]-1]++;
    start[0] = 0;
    start[1] = count[0];
    start[2] = count[0] + count[1];
}

void findWhere() {
    int i, j;
    for(i = 0; i &lt; 3; i++) {
        for(j = 0; j &lt; count[i]; j++) in[list[start[i] + j]-1][i]++;
    }
   }

void sort() {
    int h;
    // 1 &lt;-&gt; 2
    h = in[0][1];
    if(in[1][0] &lt; h) h = in[1][0];
    res += h; in[0][1] -= h; in[1][0] -= h;
    // 3 &lt;-&gt; 2
    h = in[2][1];
    if(in[1][2] &lt; h) h = in[1][2];
    res += h; in[2][1] -= h; in[1][2] -= h;
    // 1 &lt;-&gt; 3
    h = in[0][2];
    if(in[2][0] &lt; h) h = in[2][0];
    res += h; in[0][2] -= h; in[2][0] -= h;

    // Cycles
    res += (in[0][1] + in[0][2]) * 2;
}

void process() {
    findBuckets();
    findWhere();
    sort();
}

int main () {
    readFile();
    process();
    writeFile();
    return 0;
}
[/c]

标程3:
[c]
#include &lt;fstream&gt;

using namespace std;

int min (int a, int b) { return a &lt; b ? a : b; }
int max (int a, int b) { return a &gt; b ? a : b; }

int main () {
    int s[1024];
    int n;
    int sc[4] = {0};

    ifstream fin(&quot;sort3.in&quot;);
    ofstream fout(&quot;sort3.out&quot;);
    fin&gt;&gt;n;
    for (int i = 0; i &lt; n; i++) {
        fin&gt;&gt;s[i];
        sc[s[i]]++;
    }
    int s12 = 0, s13 = 0, s21 = 0, s31 = 0, s23 = 0, s32 = 0;
    for (int i = 0; i &lt; sc[1]; i++){
        if (s[i] == 2) s12++;
        if (s[i] == 3) s13++;
    }

    for (int i = sc[1]; i &lt; sc[1]+sc[2]; i++){
        if (s[i] == 1) s21++;
        if (s[i] == 3) s23++;
    }

    for (int i = sc[1]+sc[2]; i &lt; sc[1]+sc[2]+sc[3]; i++){
        if (s[i] == 1) s31++;
        if (s[i] == 2) s32++;
    }

    fout&lt;&lt;min(s12, s21)+min(s13, s31)+min(s23, s32) +
					2*(max(s12, s21) - min(s12, s21))&lt;&lt;endl;
    return 0;
}
[/c]
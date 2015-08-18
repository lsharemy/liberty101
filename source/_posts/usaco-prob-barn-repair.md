title: "USACO - \tPROB Barn Repair"
tags:
  - USACO
id: 1651
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-13 13:20:17
---

我的：<!--more-->

[c]
/*
ID: lmy0525
PROG: barn1
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
#include &lt;vector&gt;

using namespace std;

vector &lt;int&gt; occupied, gaps;

int main()
{
    ofstream fout (&quot;barn1.out&quot;);
    ifstream fin (&quot;barn1.in&quot;);
    int M, S, C;
    fin &gt;&gt; M &gt;&gt; S &gt;&gt; C;
    for (int i = 0; i &lt; C; i++)
    {
        int temp;
        fin &gt;&gt; temp;
        occupied.push_back(temp);
    }
    sort (occupied.begin(), occupied.end());
    for (int i = 1; i &lt; C; i++)
    {
        int gap = occupied[i] - occupied[i-1];
        if (gap &gt; 1)
            gaps.push_back(gap-1);
    }
    sort (gaps.begin(), gaps.end());
    int res = occupied[C-1]-occupied[0]+1;
    int idx = gaps.size()-1;
    while (M &gt; 1 &amp;&amp; idx &gt;= 0)
    {
        res -= gaps[idx];
        idx--;
        M--;
    }
    fout &lt;&lt; res &lt;&lt; endl;
    return 0;
}
[/c]

标程：

[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

#define MAXSTALL 200
int hascow[MAXSTALL];

int
intcmp(const void *va, const void *vb)
{
	return *(int*)vb - *(int*)va;
}

void
main(void)
{
    FILE *fin, *fout;
    int n, m, nstall, ncow, i, j, c, lo, hi, nrun;
    int run[MAXSTALL];

    fin = fopen(&quot;barn1.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;barn1.out&quot;, &quot;w&quot;);

    assert(fin != NULL &amp;&amp; fout != NULL);

    fscanf(fin, &quot;%d %d %d&quot;, &amp;m, &amp;nstall, &amp;ncow);
    for(i=0; i&lt;ncow; i++) {
	fscanf(fin, &quot;%d&quot;, &amp;c);
	hascow[c-1] = 1;
    }

    n = 0;	/* answer: no. of uncovered stalls */

    /* count empty stalls on left */
    for(i=0; i&lt;nstall &amp;&amp; !hascow[i]; i++)
	n++;
    lo = i;

    /* count empty stalls on right */
    for(i=nstall-1; i&gt;=0 &amp;&amp; !hascow[i]; i--)
	n++;
    hi = i+1;

    /* count runs of empty stalls */
    nrun = 0;
    i = lo;
    while(i &lt; hi) {
	while(hascow[i] &amp;&amp; i&lt;hi)
	    i++;

	for(j=i; j&lt;hi &amp;&amp; !hascow[j]; j++)
	    ;

	run[nrun++] = j-i;
	i = j;
    }

    /* sort list of runs */
    qsort(run, nrun, sizeof(run[0]), intcmp);

    /* uncover best m-1 runs */
    for(i=0; i&lt;nrun &amp;&amp; i&lt;m-1; i++)
	n += run[i];

    fprintf(fout, &quot;%d\n&quot;, nstall-n);
    exit(0);
}
[/c]

思路基本一致，不过标程中计算左右空格栏有点没必要。

另外一个：

[pas]
var f:text;
    a,b:array[1..1000] of longint;
    i,m,s,c,k:longint;

procedure qsort(l,r:longint);
    var i,j,x,y:longint;
begin
     i:=l; j:=r; x:=a[(l+r) div 2];
     repeat
           while a[i]&lt;x do i:=i+1;
           while x&lt;a[j] do j:=j-1;
           if i&lt;=j then begin
                y:=a[i]; a[i]:=a[j]; a[j]:=y;
                i:=i+1;
                j:=j-1;
           end;
     until i&gt;j;
     if l&lt;j then qsort(l,j);
     if i&lt;r then qsort(i,r);
end;

procedure qsortb(l,r:longint);
    var i,j,x,y:longint;
begin
     i:=l; j:=r; x:=b[(l+r) div 2];
     repeat
           while b[i]&lt;x do i:=i+1;
           while x&lt;b[j] do j:=j-1;
           if i&lt;=j then
           begin
                y:=b[i]; b[i]:=b[j]; b[j]:=y;
                i:=i+1;
                j:=j-1;
           end;
     until i&gt;j;
     if l&lt;j then qsortb(l,j);
     if i&lt;r then qsortb(i,r);
end;

begin
     assign(f,'barn1.in');
     reset(f);
     readln(f,m,k,c);
     for i:=1 to c do readln(f,a[i]);
     qsort(1,c);
     for i:=1 to c-1 do b[i]:=a[i+1]-a[i]-1;
     qsortb(1,c-1);
     for i:=c-1 downto (c-m+1) do s:=s+b[i];
     close(f);
     assign(f,'barn1.out');
     rewrite(f);
     writeln(f,a[c]-a[1]-s+1);
     close(f);
end.
[/pas]

这个结构挺清晰。
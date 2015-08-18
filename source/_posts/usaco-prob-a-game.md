title: USACO - PROB A Game
tags:
  - USACO
id: 1867
categories:
  - wordpress
  - index.php
  - csit
date: 2012-08-14 19:51:55
---

DP，现在知道思路的情况下写DP基本能写出了，就是想出DP还需要点时间

代码：<!--more-->

[c]
/*
ID: lmy0525
PROG: game1
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
#include &lt;stack&gt;
using namespace std;

ofstream fout(&quot;game1.out&quot;);
ifstream fin(&quot;game1.in&quot;);

int board[100];
int dp[100][100];
int sum[100][100];

int main()
{
    int N;
    fin &gt;&gt; N;
    for (int i = 0; i &lt; N; i++)
        fin &gt;&gt; board[i];
    for (int i = 0; i &lt; N; i++) sum[i][i] = board[i];
    for (int i = 1; i &lt; N; i++)
    {
        for (int j = 0; j &lt; N-i; j++)
        {
            sum[j][j+i] = sum[j][j+i-1]+sum[j+i][j+i];
        }
    }
    for (int i = 0; i &lt; N; i++) dp[i][i] = board[i];
    for (int i = 1; i &lt; N; i++)
    {
        for (int j = 0; j &lt; N-i; j++)
        {
            dp[j][j+i] = sum[j][j+i] - min(dp[j+1][j+i], dp[j][j+i-1]);
        }
    }
    fout &lt;&lt; dp[0][N-1] &lt;&lt; &quot; &quot; &lt;&lt; sum[0][N-1]-dp[0][N-1] &lt;&lt; endl;
    return 0;
}
[/c]

标程1：

[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

#define MAXBOARD 100

int board[MAXBOARD];

/*
 * best and total are indexed so that (e.g.) best[i, l] refers
 * to the board of length l starting at place i.
 */
int total[MAXBOARD][MAXBOARD];
int best[MAXBOARD][MAXBOARD];

int
max(int a, int b)
{
    return a &gt; b ? a : b;
}

void
main(void)
{
    FILE *fin, *fout;
    int j, l, n;

    fin = fopen(&quot;game1.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;game1.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    fscanf(fin, &quot;%d&quot;, &amp;n);
    for(j=0; j&lt;n; j++)
        fscanf(fin, &quot;%d&quot;, &amp;board[j]);

    /* calculate subboard totals */
    for(j=0; j&lt;n; j++)
        total[j][0] = 0;

    for(l=1; l&lt;=n; l++)
    for(j=0; j+l&lt;=n; j++)
        total[j][l] = board[j] + total[j+1][l-1];

    /* calc best for boards of size one */
    for(j=0; j+1&lt;=n; j++)
        best[j][1] = board[j];

    /* calc best for bigger boards */
    for(l=2; l&lt;=n; l++)
      for(j=0; j+l&lt;=n; j++)
        best[j][l] = max(board[j]     + total[j+1][l-1] - best[j+1][l-1],
                         board[j+l-1] + total[j  ][l-1] - best[j  ][l-1]);

    fprintf(fout, &quot;%d %d\n&quot;, best[0][n], total[0][n]-best[0][n]);

    exit(0);
}
[/c]

标程2：

[c]
#include &lt;fstream&gt;

using namespace std;

#define min(a,b) ((a&lt;b)?a:b)

ifstream fin(&quot;game1.in&quot;);
ofstream fou(&quot;game1.out&quot;);

int n;
int sum[101];
int best[101][101];

void main()
{
    fin &gt;&gt; n;
    for(int i = 1; i &lt;= n; i++) {
        fin &gt;&gt; best[i][i];
        sum[i] = sum[i-1] + best[i][i];
    }

    for(int i = 1; i &lt;= n; i++)
    for(int j = 1; j+i &lt;= n; j++)
        best[j+i][j] = sum[j+i]-sum[j-1] - min(best[j+i-1][j], best[j+i][j+1]);
    fou &lt;&lt; best[n][1] &lt;&lt; &quot; &quot; &lt;&lt; sum[n] - best[n][1] &lt;&lt; endl;
}
[/c]

标程3：

[c]
#include &lt;stdio.h&gt;
#define NMAX 101

int     best[NMAX][2], t[NMAX];
int     n;

void 
readx () {
    int     i, aux;

    freopen (&quot;game1.in&quot;, &quot;r&quot;, stdin);
    scanf (&quot;%d&quot;, &amp;n);
    for (i = 1; i &lt;= n; i++) {
	scanf (&quot;%d&quot;, &amp;aux);
	t[i] = t[i - 1] + aux;
    }
    fclose (stdin);
}

inline int 
min (int x, int y) {
    return x &gt; y ? y : x;
}

void 
solve () {
    int     i, l;

    for (l = 1; l &lt;= n; l++)
	for (i = 1; i + l &lt;= n + 1; i++)
	    best[i][l%2] = t[i + l - 1] - t[i - 1] - min (best[i + 1][(l - 1) % 2],
		best[i][(l - 1) % 2]);
}

void writex () {
    freopen (&quot;game1.out&quot;, &quot;w&quot;, stdout);
    printf (&quot;%d %d\n&quot;, best[1][n % 2], t[n] - best[1][n % 2]);
    fclose (stdout);
}

int 
main () {
    readx ();
    solve ();
    writex ();
    return 0;
}
[/c]
title: USACO - PROB The Clocks
tags:
  - USACO
id: 1680
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-14 21:28:22
---

我的：<!--more-->

[c]
/*
ID: lmy0525
PROG: clocks
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;

using namespace std;

string movestr[9] = { &quot;abde&quot;, &quot;abc&quot;, &quot;bcef&quot;, &quot;adg&quot;, &quot;bdefh&quot;, &quot;cfi&quot;, &quot;degh&quot;,
                    &quot;ghi&quot;, &quot;efhi&quot; };
int moves[9][9] = {0};
int clocks[9];
int nbestmove = 0;
int bestmove[9];
void mapMoves()
{
    for (int i = 0; i &lt; 9; i++)
    {
        for (int j = 0; j &lt; movestr[i].size(); j++)
        {
            moves[i][movestr[i][j]-'a'] = 3;
        }
    }
}

void solve (int *move, int k)
{
    if (k == 9)
    {
        for (int i = 0; i &lt; 9; i++)
        {
            if (clocks[i] % 12 != 0) return;
        }

        // good
        int n = 0;
        for (int i = 0; i &lt; 9; i++)
        {
            n += move[i];
        }
        if (nbestmove == 0 || n &lt; nbestmove)
        {
            nbestmove = n;
            for (int i = 0; i &lt; 9; i++) bestmove[i] = move[i];
        }
        return;
    }

    for (int rep = 3; rep &gt;= 0; rep--)
    {
        for (int i = 0; i &lt; rep; i++)
        {
            for (int j = 0; j &lt; 9; j++)
            {
                clocks[j] += moves[k][j];
            }
        }
        move[k] = rep;

        solve(move, k+1);

        for (int i = 0; i &lt; rep; i++)
        {
            for (int j = 0; j &lt; 9; j++)
            {
                clocks[j] -= moves[k][j];
            }
        }
    }
}

int main() {
    ofstream fout (&quot;clocks.out&quot;);
    ifstream fin (&quot;clocks.in&quot;);
    mapMoves();
    for (int i = 0; i &lt; 9; i++)
    {
        fin &gt;&gt; clocks[i];
    }
    int solution[9];
    solve(solution, 0);
    int first = 1;
    for (int i = 0; i &lt; 9; i++)
    {
        for (int j = 0; j &lt; bestmove[i]; j++)
        {
            if (first == 1) first = 0;
            else fout &lt;&lt; &quot; &quot;;
            fout &lt;&lt; i+1;
        }
    }
    fout &lt;&lt; endl;
    return 0;
}
[/c]

还是考察的DFS咯，现在算是知道了，回溯法其实就是DFS呢= =

标程：

[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;
#include &lt;ctype.h&gt;

#define INF 60000	/* bigger than longest possible path */

char *movestr[] = { &quot;abde&quot;, &quot;abc&quot;, &quot;bcef&quot;, &quot;adg&quot;, &quot;bdefh&quot;, &quot;cfi&quot;, &quot;degh&quot;,
                    &quot;ghi&quot;, &quot;efhi&quot; };

int movedist[9][9];
int clock[9];

int bestmove[9];
int nbestmove;

/* translate move strings into array &quot;movedist&quot; of which clocks change on each move */
void
mkmove(void)
{
    int i;
    char *p;

    for(i=0; i&lt;9; i++)
	for(p=movestr[i]; *p; p++)
	    movedist[i][*p-'a'] = 3;
}

/* apply some number of each move from k to 9 */
/* move contains the number of times each move is applied */
void
solve(int *move, int k)
{
    int i, j, n, rep;

    if(k == 9) {
	for(j=0; j&lt;9; j++)
	    if(clock[j]%12 != 0)
		return;

	/* we have a successful sequence of moves */
	n = 0;
	for(j=0; j&lt;9; j++)
	    n += move[j];

	if(nbestmove == 0 || n &lt; nbestmove) {
	    nbestmove = n;
	    for(j=0; j&lt;9; j++)
		bestmove[j] = move[j];
	}
	return;
    }

    /*
     * the for loop counts down so we
     * generate smaller numbers first by
     * trying more of small numbered
     * moves before we try less of them.
     */
    for(rep=3; rep&gt;=0; rep--) {
	/* apply move k rep times */
	for(i=0; i&lt;rep; i++)
	    for(j=0; j&lt;9; j++)
		clock[j] += movedist[k][j];
	move[k] = rep;

	solve(move, k+1);

	/* undo move */
	for(i=0; i&lt;rep; i++)
	    for(j=0; j&lt;9; j++)
		clock[j] -= movedist[k][j];
    }
}

void
main(void)
{
    FILE *fin, *fout;
    int i, j, move[9];
    char *sep;

    fin = fopen(&quot;clocks.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;clocks.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    mkmove();

    for(i=0; i&lt;9; i++)
	fscanf(fin, &quot;%d&quot;, &amp;clock[i]);

    solve(move, 0);

    sep = &quot;&quot;;
    for(i=0; i&lt;9; i++) {
	for(j=0; j&lt;bestmove[i]; j++) {
	    fprintf(fout, &quot;%s%d&quot;, sep, i+1);
	    sep = &quot; &quot;;
	}
    }
    fprintf(fout, &quot;\n&quot;);

    exit(0);
}
[/c]

更好的解法：

[c]
#include &lt;stdio.h&gt;

int a[9][9]= { {3,3,3,3,3,2,3,2,0},
               {2,3,2,3,2,3,1,0,1},
               {3,3,3,2,3,3,0,2,3},
               {2,3,1,3,2,0,2,3,1},
               {2,3,2,3,1,3,2,3,2},
               {1,3,2,0,2,3,1,3,2},
               {3,2,0,3,3,2,3,3,3},
               {1,0,1,3,2,3,2,3,2},
               {0,2,3,2,3,3,3,3,3} };
int v[9];

int main() {
    int i,j,k;
    freopen(&quot;clocks.in&quot;,&quot;r&quot;,stdin);
    for (i=0; i&lt;9; i++) {
        scanf(&quot;%d&quot;,&amp;k);
        for(j=0; j&lt;9; j++)
             v[j]=(v[j]+(4-k/3)*a[i][j])%4;
    }
    fclose(stdin);

    k=0;
    freopen(&quot;clocks.out&quot;,&quot;w&quot;,stdout);
    for (i=0; i&lt;9; i++)
        for (j=0; j&lt;v[i]; j++)
            if (!k) { printf(&quot;%d&quot;,i+1); k=1; }
            else    printf(&quot; %d&quot;,i+1);
    printf(&quot;\n&quot;);
    fclose(stdout);
    return 0;
}
[/c]

更变态的解法：

[c]
program clocks;
const
  INV : array[3..12] of byte =(1, 0, 0, 2, 0, 0, 3, 0, 0, 0);

var inp, out: text;
    a, b, c, d, e, f, g, h, i: integer;
    ax, bx, cx, dx, ex, fx, gx, hx, ix,l: integer;
    t,an: array[1..9] of integer;
begin
    assign (inp, 'clocks.in'); reset (inp);
    readln(inp, ax, bx, cx);
    readln(inp, dx, ex, fx);
    readln(inp, gx, hx, ix);
    a:=inv[ax]; b:=inv[bx]; c:=inv[cx]; d:=inv[dx];
    e:=inv[ex]; f:=inv[fx]; g:=inv[gx]; h:=inv[hx];
    i:=inv[ix];
    t[1] := (8+a+2*b+c+2*d+2*e-f+g-h) mod 4;
    t[2] := (a+b+c+d+e+f+2*g+    2*i) mod 4;
    t[3] := (8+  a+2*b+  c  -d+2*e+2*f      -h+  i) mod 4;
    t[4] := (    a+  b+2*c+  d+  e+      g+  h+2*i) mod 4;
    t[5] := (4+  a+2*b+  c+2*d  -e+2*f+  g+2*h+  i) mod 4;
    t[6] := (  2*a+  b+  c+      e+  f+2*g+  h+  i) mod 4;
    t[7] := (8+  a  -b+    2*d+2*e  -f+  g+2*h+  i) mod 4;
    t[8] := (  2*a+    2*c+  d+  e+  f+  g+  h+  i) mod 4;
    t[9] := (8      -b+  c  -d+2*e+2*f+  g+2*h+  i) mod 4;
    assign(out, 'clocks.out'); rewrite(out);
    for a := 1 to 9 do
	for b := 1 to t[a] do Begin
	    inc(l);
	    an[l]:=a;
	end;
    for a:=1 to l-1 do
	write(out,an[a],' ');
    write(out,an[l]);
    writeln(out); close(out)
end.
[/c]
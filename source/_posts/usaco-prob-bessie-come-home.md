title: USACO - PROB Bessie Come Home
tags:
  - USACO
id: 1787
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-21 21:39:29
---

Floyd敲起来方便，所以继续用。因为只有50几个顶点。

代码：<!--more-->
[c]
/*
ID: lmy0525
PROG: comehome
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
using namespace std;

ofstream fout;
ifstream fin;

int P;
int dist[52][52];

int tonum(char c)
{
    if (isupper(c))
        return c-'A';
    else
        return c-'a'+26;
}

int main()
{
    fout.open(&quot;comehome.out&quot;);
    fin.open(&quot;comehome.in&quot;);
    fin &gt;&gt; P;
    memset (dist, 0x25252525, sizeof(dist));

    for (int i = 0; i &lt; 52; i++) dist[i][i] = 0;
    for (int i = 0; i &lt; P; i++)
    {
        char c1, c2;
        int d;
        fin &gt;&gt; c1 &gt;&gt; c2 &gt;&gt; d;
        int a1 = tonum(c1);
        int a2 = tonum(c2);
        dist[a1][a2] = dist[a2][a1] = min(d, dist[a1][a2]);
    }

    for (int k = 0; k &lt; 52; k++)
    for (int i = 0; i &lt; 52; i++)
    for (int j = 0; j &lt; 52; j++)
    {
        if (dist[i][k] + dist[k][j] &lt; dist[i][j])
            dist[i][j] = dist[i][k] + dist[k][j];
    }

    char firstc;
    int firstd = 0x25252525;
    for (int i = 0; i &lt; 25; i++)
    {
        if (dist[i][25] &lt; firstd)
        {
            firstd = dist[i][25];
            firstc = i+'A';
        }
    }
    fout &lt;&lt; firstc &lt;&lt; &quot; &quot; &lt;&lt; firstd &lt;&lt; endl;
    return 0;
}
[/c]

标程：
[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;
#include &lt;ctype.h&gt;

#define INF 60000	/* bigger than longest possible path */

int dist[52][52];

int
char2num(char c)
{
    assert(isalpha(c));

    if(isupper(c))
	return c-'A';
    else
	return c-'a'+26;
}

void
main(void)
{
    FILE *fin, *fout;
    int i, j, k, npath, d;
    char a, b;
    int m;

    fin = fopen(&quot;comehome.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;comehome.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    for(i=0; i&lt;52; i++)
    for(j=0; j&lt;52; j++)
	dist[i][j] = INF;

    for(i=0; i&lt;26; i++)
	dist[i][i] = 0;

    fscanf(fin, &quot;%d\n&quot;, &amp;npath);
    for(i=0; i&lt;npath; i++) {
	fscanf(fin, &quot;%c %c %d\n&quot;, &amp;a, &amp;b, &amp;d);
	a = char2num(a);
	b = char2num(b);
	if(dist[a][b] &gt; d)
	    dist[a][b] = dist[b][a] = d;
    }

    /* floyd warshall all pair shortest path */
    for(k=0; k&lt;52; k++)
    for(i=0; i&lt;52; i++)
    for(j=0; j&lt;52; j++)
	if(dist[i][k]+dist[k][j] &lt; dist[i][j])
	    dist[i][j] = dist[i][k]+dist[k][j];

    /* find closest cow */
    m = INF;
    a = '#';
    for(i='A'; i&lt;='Y'; i++) {
	d = dist[char2num(i)][char2num('Z')];
	if(d &lt; m) {
	    m = d;
	    a = i;
	}
    }

    fprintf(fout, &quot;%c %d\n&quot;, a, m);
    exit(0);
}
[/c]

标程（dijstra）：
[pascal]
Var Dist:Array [1..58] of LongInt;      {Array with distances to barn}
    Vis :Array [1..58] of Boolean;      {Array keeping track which
pastures visited}
    Conn:Array [1..58,1..58] of Word;   {Matrix with length of edges, 0 = no edge}

Procedure Load;
Var TF   :Text;
    X,D,E:Word;
    P1,P2:Char;

Begin
 Assign(TF,'comehome.in');
 Reset(TF);
 Readln(TF,E);                          {Read number of edges}
 For X:=1 to E do
 Begin
  Read(TF,P1);                          {Read both pastures and edge
length}
  Read(TF,P2);
  Read(TF,P2);      {Add edge in matrix if no edge between P1 and P2 yet or}
  Readln(TF,D);     {this edge is shorter than the shortest till now}
  If (Conn[Ord(P1)-Ord('A')+1,Ord(P2)-Ord('A')+1]=0) or
     (Conn[Ord(P1)-Ord('A')+1,Ord(P2)-Ord('A')+1]&gt;D) then
  Begin
   Conn[Ord(P1)-Ord('A')+1,Ord(P2)-Ord('A')+1]:=D;
   Conn[Ord(P2)-Ord('A')+1,Ord(P1)-Ord('A')+1]:=D;
  End;
 End;
 Close(TF);
 For X:=1 to 58 do
  Dist[X]:=2147483647;                  {Set all distances to infinity}
 Dist[Ord('Z')-Ord('A')+1]:=0;          {Set distance from barn to barn to 0}
End;

Procedure Solve;
Var X,P,D:LongInt;                      {P = pasture and D = distance}

Begin
 Repeat
  P:=0;
  D:=2147483647;
  For X:=1 to 58 do                     {Find nearest pasture not
visited yet}
   If Not Vis[X] and (Dist[X]&lt;D) then
   Begin
    P:=X;
    D:=Dist[X];
   End;
  If (P&lt;&gt;0) then
  Begin
   Vis[P]:=True;                        {If there is one mark it
visited}
   For X:=1 to 58 do                    {And update all distances}
    If (Conn[P,X]&lt;&gt;0) and (Dist[X]&gt;Dist[P]+Conn[P,X]) then
     Dist[X]:=Dist[P]+Conn[P,X];
  End;
 Until (P=0);                {Until no reachable and unvisited pastures
left}
End;

Procedure Save;
Var TF  :Text;
    X,BD:LongInt;                       {BD = best distance}
    BP  :Char;                          {BP = best pasture}

Begin
 BD:=2147483647;
 For X:=1 to 25 do                      {Find neares pasture}
  If (Dist[X]&lt;BD) then
  Begin
   BD:=Dist[X];
   BP:=Chr(Ord('A')+X-1);
  End;
 Assign(TF,'comehome.out');
 Rewrite(TF);
 Writeln(TF,BP,' ',BD);                 {Write outcome to disk}
 Close(TF);
End;

Begin
 Load;
 Solve;
 Save;
End.
[/pascal]
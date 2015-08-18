title: "USACO - PROB Mother's Milk"
tags:
  - USACO
id: 1695
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-16 16:27:43
---

我的：<!--more-->

[c]
/*
ID: lmy0525
PROG: milk3
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;vector&gt;
#include &lt;algorithm&gt;

using namespace std;

int Capacity[3];
int ab[21][21] = {0};
vector &lt;int&gt; c;
int d[6][2] = { {0, 1}, {1, 0}, {0, 2}, {2, 0}, {1, 2}, {2, 1} };

void pour(int *t, int s, int d)
{
    int pourmilk = min(t[s], (Capacity[d]-t[d]));
    t[s] = t[s] - pourmilk;
    t[d] = t[d] + pourmilk;
}

void dfs(int* t)
{
    ab[t[0]][t[1]] = 1;
    if (t[0] == 0) c.push_back(t[2]);

    for (int i = 0; i &lt; 6; i++)
    {
        int temp[3];
        for (int j = 0; j &lt; 3; j++) temp[j] = t[j];
        pour(temp, d[i][0], d[i][1]);
        if (ab[temp[0]][temp[1]] == 0)
        {
            dfs(temp);
        }
    }
}

int main() {
    ofstream fout (&quot;milk3.out&quot;);
    ifstream fin (&quot;milk3.in&quot;);
    fin &gt;&gt; Capacity[0] &gt;&gt; Capacity[1] &gt;&gt; Capacity[2];
    int temp[3];
    temp[0] = 0, temp[1] = 0, temp[2] = Capacity[2];
    dfs(temp);
    sort (c.begin(), c.end());
    for (int i = 0; i &lt; c.size(); i++)
    {
        if (i != 0) fout &lt;&lt; &quot; &quot;;
        fout &lt;&lt; c[i];
    }
    fout &lt;&lt; endl;
    return 0;
}
[/c]

又没想到用结构体= =

标程：

[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;
#include &lt;ctype.h&gt;

#define MAX 20

typedef struct State	State;
struct State {
    int a[3];
};

int seen[MAX+1][MAX+1][MAX+1];
int canget[MAX+1];

State
state(int a, int b, int c)
{
    State s;

    s.a[0] = a;
    s.a[1] = b;
    s.a[2] = c;
    return s;
}

int cap[3];

/* pour from bucket &quot;from&quot; to bucket &quot;to&quot; */
State
pour(State s, int from, int to)
{
    int amt;

    amt = s.a[from];
    if(s.a[to]+amt &gt; cap[to])
	amt = cap[to] - s.a[to];

    s.a[from] -= amt;
    s.a[to] += amt;
    return s;
}

void
search(State s)
{
    int i, j;

    if(seen[s.a[0]][s.a[1]][s.a[2]])
	return;

    seen[s.a[0]][s.a[1]][s.a[2]] = 1;

    if(s.a[0] == 0)	/* bucket A empty */
	canget[s.a[2]] = 1;

    for(i=0; i&lt;3; i++)
    for(j=0; j&lt;3; j++)
	search(pour(s, i, j));
}

void
main(void)
{
    int i;
    FILE *fin, *fout;
    char *sep;

    fin = fopen(&quot;milk3.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;milk3.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    fscanf(fin, &quot;%d %d %d&quot;, &amp;cap[0], &amp;cap[1], &amp;cap[2]);

    search(state(0, 0, cap[2]));

    sep = &quot;&quot;;
    for(i=0; i&lt;=cap[2]; i++) {
	if(canget[i]) {
	    fprintf(fout, &quot;%s%d&quot;, sep, i);
	    sep = &quot; &quot;;
	}
    }
    fprintf(fout, &quot;\n&quot;);

    exit(0);
}
[/c]

DP:

[c]
#include&lt;stdio.h&gt;

int m[21][21][21];
int poss[21];
int A, B, C;

int main(void) {
    int i,j,k;
    int flag;
    FILE* in=fopen(&quot;milk3.in&quot;,&quot;r&quot;);
    fscanf(in, &quot;%d %d %d&quot;,&amp;A, &amp;B, &amp;C);
    fclose(in);
    for(i=0;i&lt;21;i++)
        for(j=0;j&lt;21;j++)
            for(k=0;k&lt;21;k++)
                m[i][j][k]=0;
    for(i=0;i&lt;21;i++)
        poss[i]=0;
    m[0][0][C]=1;

    for(flag=1;flag;) {
        flag=0;
        for(i=0;i&lt;=A;i++)
            for(j=0;j&lt;=B;j++)
                for(k=0;k&lt;=C;k++) {
                    if(m[i][j][k]) {
                	if(i==0) poss[k]=1;
		        if(i) {
	                    if(j&lt;B) {
                                if(B-j&gt;=i) {
                            	    if( m[0][j+i][k]==0) {
                                        m[0][j+i][k]=1;
                                	flag=1;
                            	    }
                                } else {
                            	    if( m[i-(B-j)][B][k] == 0) {
                                        m[i-(B-j)][B][k] =1;
                                        flag=1;
                                    }
                                }
                            }
                            if(k&lt;C) {
                                if(C-k&gt;=i) {
                                    if( m[0][j][k+i]==0) {
                                        m[0][j][k+i]=1;
                                        flag=1;
                                    }
                                }
                                else {
                                    if( m[i-(C-k)][j][C] == 0) {
                                        m[i-(C-k)][j][C] =1;
                                        flag=1;
                                    }
                                }
                            }
                        }
                        if(j) {
                            if(i&lt;A) {
                                if(A-i&gt;=j) {
                                    if( m[i+j][0][k]==0) {
                                        m[i+j][0][k]=1;
                                        flag=1;
                                    }
                                } else {
                                    if( m[A][j-(A-i)][k] == 0) {
                                        m[A][j-(A-i)][k] =1;
                                        flag=1;
                                    }
                                }
                            }
                            if(k&lt;C) {
                                if(C-k&gt;=j) {
                                    if( m[i][0][k+j]==0) {
                                        m[i][0][k+j]=1;
                                        flag=1;
                                    }
                                } else {
                                    if( m[i][j-(C-k)][C] == 0) {
                                        m[i][j-(C-k)][C] =1;
                                        flag=1;
                                    }
                                }
                            }
                        }
                        if(k) {
                            if(i&lt;A) {
                                if(A-i&gt;=k) {
                                    if( m[i+k][j][0]==0) {
                                        m[i+k][j][0]=1;
                                        flag=1;
                                    }
                                } else {
                                    if( m[A][j][k-(A-i)] == 0) {
                                        m[A][j][k-(A-i)] =1;
                                        flag=1;
                                    }
                                }
                            }
                            if(j&lt;B) {
                                if(B-j&gt;=k) {
                                    if( m[i][j+k][0]==0) {
                                        m[i][j+k][0]=1;
                                        flag=1;
                                    }
                                } else {
                                    if( m[i][B][k-(B-j)] == 0) {
                                        m[i][B][k-(B-j)] =1;
                                        flag=1;
                                    }
                                }
                            }
                        }
            }                   
        }
    }
    {
        FILE* out=fopen(&quot;milk3.out&quot;, &quot;w&quot;);
        for(i=0;i&lt;21;i++) {
            if(poss[i]) {
                fprintf(out,&quot;%d&quot;,i);
                i++;
                break;
            }
        }
        for(;i&lt;21;i++) {
            if(poss[i]) {
                fprintf(out, &quot; %d&quot;, i);
            }
        }
        fprintf(out,&quot;\n&quot;);
    }
    return 0;
}
[/c]
用两个变量表示状态：

[c]
#include &lt;stdio.h&gt;
int A, B, C;
int CB[21][21]; // All states

void readFile() {
    FILE *f;
    f = fopen(&quot;milk3.in&quot;, &quot;r&quot;);
    fscanf(f, &quot;%d%d%d&quot;, &amp;A, &amp;B, &amp;C);
    fclose(f);
}

void writeFile() {
    FILE *f; int i;
    f = fopen(&quot;milk3.out&quot;, &quot;w&quot;);
    for(i = 0; i &lt;= C; i++) {
        if(CB[i][C - i] == 1) {
            if((i != C-B) &amp;&amp; (i != 0)) fprintf(f, &quot; &quot;);
            fprintf(f, &quot;%d&quot;, i);
        }
    }
    fprintf(f, &quot;\n&quot;);
    fclose(f);
}

// do brute-force search, c/b: current state
void search(int c, int b) {
    int a;
    if(CB[c][b] == 1) return; // already searched
    CB[c][b] = 1;
    a = C-b-c; // calc amount in A
    // do all moves:
    // c-&gt;b
    if(B &lt; c+b) search(c - (B - b), B);
    else search(0, c + b);
    // b-&gt;c
    if(C &lt; c+b) search(C, b - (C - c));
    else search(c + b, 0);
    // c-&gt;a
    if(A &lt; c+a) search(c - (A - a), b);
    else search(0, b);
    // a-&gt;c
    if(C &lt; c+a) search(C, b);
    else search(c + a, b);
    // b-&gt;a
    if(A &lt; b+a) search(c, b - (A - a));
    else search(c, 0);
    // a-&gt;b
    if(B &lt; b+a) search(c, B);
    else search(c, b + a);
   }

int main () {
    readFile();
    search(C, 0);
    writeFile();
    return 0;
}
[/c]
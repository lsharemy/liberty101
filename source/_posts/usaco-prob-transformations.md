title: "USACO - \tPROB Transformations"
tags:
  - USACO
id: 1617
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-12 18:25:00
---

我的：（其实这个有点不好意思贴了，忽忽）<!--more-->

[c]
/*
ID: lmy0525
PROG: transform
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;

using namespace std;

int N; // 1 - 10
string square1[10];
string square2[10];

bool transform1()
{
    bool flag = true;
    for (int i = 0; i &lt; N; i++)
    {
        for (int j = 0; j &lt; N; j++)
        {
            int r = j;
            int c = N-i-1;
            if (square2[r][c] != square1[i][j])
            {
                flag = false;
                break;
            }
        }
    }
    return flag;
}

bool transform2()
{
    bool flag = true;
    for (int i = 0; i &lt; N; i++)
    {
        for (int j = 0; j &lt; N; j++)
        {
            int r = N-i-1;
            int c = N-j-1;
            if (square2[r][c] != square1[i][j])
            {
                flag = false;
                break;
            }
        }
    }
    return flag;
}

bool transform3()
{
    bool flag = true;
    for (int i = 0; i &lt; N; i++)
    {
        for (int j = 0; j &lt; N; j++)
        {
            int r = N-j-1;
            int c = i;
            if (square2[r][c] != square1[i][j])
            {
                flag = false;
                break;
            }
        }
    }
    return flag;
}

bool transform4()
{
    bool flag = true;
    for (int i = 0; i &lt; N; i++)
    {
        for (int j = 0; j &lt; N; j++)
        {
            int r = i;
            int c = N-j-1;
            if (square2[r][c] != square1[i][j])
            {
                flag = false;
                break;
            }
        }
    }
    return flag;
}

bool transform5()
{
    bool flag = true;
    for (int i = 0; i &lt; N; i++)
    {
        for (int j = 0; j &lt; N; j++)
        {
            int r = N-j-1;
            int c = N-i-1;
            if (square2[r][c] != square1[i][j])
            {
                flag = false;
                break;
            }
        }
    }
    if (!flag)
    {
        flag = true;
        for (int i = 0; i &lt; N; i++)
        {
            for (int j = 0; j &lt; N; j++)
            {
                int r = N-i-1;
                int c = j;
                if (square2[r][c] != square1[i][j])
                {
                    flag = false;
                    break;
                }
            }
        }
    }
    if (!flag)
    {
        flag = true;
        for (int i = 0; i &lt; N; i++)
        {
            for (int j = 0; j &lt; N; j++)
            {
                int r = j;
                int c = i;
                if (square2[r][c] != square1[i][j])
                {
                    flag = false;
                    break;
                }
            }
        }
    }
    return flag;
}

bool transform6()
{
    bool flag = true;
    for (int i = 0; i &lt; N; i++)
    {
        for (int j = 0; j &lt; N; j++)
        {
            int r = i;
            int c = j;
            if (square2[r][c] != square1[i][j])
            {
                flag = false;
                break;
            }
        }
    }
    return flag;
}
int main()
{
    ofstream fout (&quot;transform.out&quot;);
    ifstream fin (&quot;transform.in&quot;);
    fin &gt;&gt; N;
    for (int i = 0; i &lt; N; i++)
    {
        fin &gt;&gt; square1[i];
    }
    for (int i = 0; i &lt; N; i++)
    {
        fin &gt;&gt; square2[i];
    }
    if (transform1())
    {
        fout &lt;&lt; 1 &lt;&lt; endl;
        return 0;
    }
    else if (transform2())
    {
        fout &lt;&lt; 2 &lt;&lt; endl;
        return 0;
    }
    else if (transform3())
    {
        fout &lt;&lt; 3 &lt;&lt; endl;
        return 0;
    }
    else if (transform4())
    {
        fout &lt;&lt; 4 &lt;&lt; endl;
        return 0;
    }
    else if (transform5())
    {
        fout &lt;&lt; 5 &lt;&lt; endl;
        return 0;
    }
    else if (transform6())
    {
        fout &lt;&lt; 6 &lt;&lt; endl;
        return 0;
    }
    fout &lt;&lt; 7 &lt;&lt; endl;
    return 0;
}
[/c]

标程：

[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

#define MAXN 10

typedef struct Board Board;
struct Board {
    int n;
    char b[MAXN][MAXN];
};

/* rotate 90 degree clockwise: [r, c] -&gt; [c, n+1 - r] */
Board
rotate(Board b)
{
    Board nb;
    int r, c;

    nb = b;
    for(r=0; r&lt;b.n; r++)
    for(c=0; c&lt;b.n; c++)
        nb.b[c][b.n+1 - r] = b.b[r][c];

    return nb;
}

/* reflect board horizontally: [r, c] -&gt; [r, n-1 -c] */
Board
reflect(Board b)
{
    Board nb;
    int r, c;

    nb = b;
    for(r=0; r&lt;b.n; r++)
    for(c=0; c&lt;b.n; c++)
        nb.b[r][b.n-1 - c] = b.b[r][c];

    return nb;
}

/* return non-zero if and only if boards are equal */
int
eqboard(Board b, Board bb)
{
    int r, c;

    if(b.n != bb.n)
        return 0;

    for(r=0; r&lt;b.n; r++)
    for(c=0; c&lt;b.n; c++)
        if(b.b[r][c] != bb.b[r][c])
            return 0;
    return 1;
}

Board
rdboard(FILE *fin, int n)
{
    Board b;
    int r, c;

    b.n = n;
    for(r=0; r&lt;n; r++) {
        for(c=0; c&lt;n; c++)
            b.b[r][c] = getc(fin);
        assert(getc(fin) == '\n');
    }
    return b;
}

void
main(void)
{
    FILE *fin, *fout;
    Board b, nb;
    int n, change;

    fin = fopen(&quot;transform.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;transform.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    fscanf(fin, &quot;%d\n&quot;, &amp;n);
    b = rdboard(fin, n);
    nb = rdboard(fin, n);

    if(eqboard(nb, rotate(b)))
        change = 1;
    else if(eqboard(nb, rotate(rotate(b))))
        change = 2;
    else if(eqboard(nb, rotate(rotate(rotate(b)))))
        change = 3;
    else if(eqboard(nb, reflect(b)))
        change = 4;
    else if(eqboard(nb, rotate(reflect(b)))
         || eqboard(nb, rotate(rotate(reflect(b))))
         || eqboard(nb, rotate(rotate(rotate(reflect(b))))))
        change = 5;
    else if(eqboard(nb, b))
        change = 6;
    else
        change = 7;

    fprintf(fout, &quot;%d\n&quot;, change);

    exit(0);
}
[/c]

之前的想法就是这样做的，但没想到这么用结构体，多次相同的输入也可以写成个函数，哎，我太弱了
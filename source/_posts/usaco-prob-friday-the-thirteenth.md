title: USACO - PROB Friday the Thirteenth
tags:
  - USACO
id: 1593
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-11 18:35:15
---

我的：<!--more-->

[c]
/*
ID: lmy0525
LANG: C++
PROG: friday
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;map&gt;
#include &lt;cassert&gt;

using namespace std;

int mouth[] = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};

bool isLeap(int year)
{
    if (year % 400 == 0)
        return true;
    else if (year % 100 == 0)
        return false;
    else if (year % 4 == 0)
        return true;
    else
        return false;
}

int main()
{
    ofstream fout (&quot;friday.out&quot;);
    ifstream fin (&quot;friday.in&quot;);
    int N; // 0 - 400
    int res[7] = {0};
    fin &gt;&gt; N;
    int days = 13;
    for (int year = 1900; year &lt; 1900 + N; year++)
    {
        for (int i = 0; i &lt; 12; i++)
        {
            res[(days+1)%7]++;
            if (i == 1) // February
            {
                if (isLeap(year))
                {
                    days++;
                }
            }
            days += mouth[i];
        }
    }
    for (int i = 0; i &lt; 7; i++)
    {
        if (i != 0) fout &lt;&lt; &quot; &quot;;
        fout &lt;&lt; res[i];
    }
    fout &lt;&lt; endl;
    return 0;
}
[/c]

标程：

[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

int
isleap(int y)
{
    return y%4==0 &amp;&amp; (y%100 != 0 || y%400 == 0);
}

int mtab[] = { 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };

/* return length of month m in year y */
int
mlen(int y, int m)
{
    if(m == 1)    /* february */
        return mtab[m]+isleap(y);
    else
        return mtab[m];
}

void
main(void)
{
    FILE *fin, *fout;
    int i, m, dow, n, y;
    int ndow[7];

    fin = fopen(&quot;friday.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;friday.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    fscanf(fin, &quot;%d&quot;, &amp;n);

    for(i=0; i&lt;7; i++)
        ndow[i] = 0;

    dow = 0;    /* day of week: January 13, 1900 was a Saturday = 0 */
    for(y=1900; y&lt;1900+n; y++) {
        for(m=0; m&lt;12; m++) {
            ndow[dow]++;
            dow = (dow+mlen(y, m)) % 7;
        }
    }

    for(i=0; i&lt;7; i++) {
        if(i)
            fprintf(fout, &quot; &quot;);
        fprintf(fout, &quot;%d&quot;, ndow[i]);
    }
    fprintf(fout, &quot;\n&quot;);

    exit(0);
}
[/c]

isleap一行就够了，还有就是计算一个月有几天写成一个函数，会使程序更加清晰
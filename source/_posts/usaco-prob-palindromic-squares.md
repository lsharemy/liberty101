title: USACO - PROB Palindromic Squares
tags:
  - USACO
id: 1621
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-12 21:07:24
---

我的：<!--more-->

[c]
/*
ID: lmy0525
PROG: palsquare
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;

using namespace std;

string NtoA[] = {&quot;0&quot;, &quot;1&quot;, &quot;2&quot;, &quot;3&quot;, &quot;4&quot;, &quot;5&quot;,
           &quot;6&quot;, &quot;7&quot;, &quot;8&quot;, &quot;9&quot;, &quot;A&quot;, &quot;B&quot;,
           &quot;C&quot;, &quot;D&quot;, &quot;E&quot;, &quot;F&quot;, &quot;G&quot;, &quot;H&quot;,
           &quot;I&quot;, &quot;J&quot;};

int base;

string tobase(int num)
{
    string temp = &quot;&quot;+NtoA[num%base];
    while (num / base)
    {
        num /= base;
        temp = NtoA[num%base]+temp;
    }
    return temp;
}

bool isPals(string s)
{
    int n = s.size();
    int h = n/2;
    for (int i = 0; i &lt;= h; i++)
    {
        if (s[i] != s[n-i-1])
        {
            return false;
        }
    }
    return true;
}

int main() {
    ofstream fout (&quot;palsquare.out&quot;);
    ifstream fin (&quot;palsquare.in&quot;);
    fin &gt;&gt; base;
    for (int i = 1; i &lt;= 300; i++)
    {
        int num2 = i*i;
        string snum2 = tobase(num2);
        string snum = tobase(i);
        if (isPals(snum2))
        {
            fout &lt;&lt; snum &lt;&lt; &quot; &quot; &lt;&lt; snum2 &lt;&lt; endl;
        }
    }
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
#include &lt;math.h&gt;

/* is string s a palindrome? */
int
ispal(char *s)
{
    char *t;

    t = s+strlen(s)-1;
    for(t=s+strlen(s)-1; s&lt;t; s++, t--)
    	if(*s != *t)
	    return 0;

    return 1;
}

/* put the base b representation of n into s: 0 is represented by &quot;&quot; */

void
numbconv(char *s, int n, int b)
{
    int len;

    if(n == 0) {
	strcpy(s, &quot;&quot;);
	return;
    }

    /* figure out first n-1 digits */
    numbconv(s, n/b, b);

    /* add last digit */
    len = strlen(s);
    s[len] = &quot;0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ&quot;[n%b];
    s[len+1] = '&#92;&#48;';
}

void
main(void)
{
    char s[20];
    char t[20];
    int i, base;
    FILE *fin, *fout;

    fin = fopen(&quot;palsquare.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;palsquare.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    fscanf(fin, &quot;%d&quot;, &amp;base);
    for(i=1; i &lt;= 300; i++) {
	numbconv(s, i*i, base);
	if(ispal(s)) {
	    numbconv(t, i, base);
	    fprintf(fout, &quot;%s %s\n&quot;, t, s);
	}
    }
    exit(0);
}
[/c]

这回程序终于和标程写的类似了，忽忽

原来还可以这样："0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ"[n%b];
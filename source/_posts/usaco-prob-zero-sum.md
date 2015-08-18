title: USACO - PROB Zero Sum
tags:
  - USACO
id: 1772
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-20 21:49:38
---

DFS所有情况，然后计算和是否为0.

我的：<!--more-->
[c]
/*
ID: lmy0525
PROG: zerosum
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
#include &lt;cmath&gt;
#include &lt;vector&gt;

using namespace std;

char op[8];

ofstream fout;
ifstream fin;

void generate(int cur, int n)
{
    if (cur == n)
    {
        // calc
        int a = 1, b = 0;
        char op_pre;
        for (int i = 0; i &lt; n; i++)
        {
            if (b == 0)
            {
                if (op[i] == ' ')
                {
                    a = a*10 + (i+2);
                }
                else
                {
                    b = i+2; 
                    op_pre = op[i];
                }
            }
            else
            {
                if (op[i] == ' ')
                {
                    b = b*10+(i+2);
                }
                else
                {
                    if (op_pre == '+') a += b;
                    else a -= b;

                    b = i+2;
                    op_pre = op[i];
                }
            }
        }
        // last
        if (op_pre == '+') a += b;
        else a -= b;

        if (a == 0)
        {
            fout &lt;&lt; 1;
            for (int i = 0; i &lt; n; i++)
            {
                fout &lt;&lt; op[i] &lt;&lt; i+2;
            }
            fout &lt;&lt; endl;
        }
        return;
    }

    op[cur] = ' ';
    generate(cur+1, n);

    op[cur] = '+';
    generate(cur+1, n);

    op[cur] = '-';
    generate(cur+1, n);
}

int main() {
    fout.open(&quot;zerosum.out&quot;);
    fin.open(&quot;zerosum.in&quot;);
    int N;
    fin &gt;&gt; N;
    generate(0, N-1);
    return 0;
}
[/c]

标程：
[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

FILE *fout;
int n;

/* evaluate the string s as arithmetic and return the sum */
int
eval(char *s)
{
    int term, sign, sum;
    char *p;

    sign = 1;
    term = 0;
    sum = 0;
    for(p=s; *p; p++) {
        switch(*p){
        case '+':
        case '-':
            sum += sign*term;
            term = 0;
            sign = *p == '+' ? 1 : -1;
            break;
        case ' ':
            break;
        default:    /* digit */
            term = term*10 + *p-'0';
        }
    }
    sum += sign*term;
    return sum;
}

/* 
 * Insert + - or space after each number, and then  
 * test to see if we get zero.  The first k numbers have
 * already been taken care of.
 */
void
search(char *s, int k)
{
    char *p;

    if(k == n-1) {
        if(eval(s) == 0)
            fprintf(fout, &quot;%s\n&quot;, s);
        return;
    }

    for(p=&quot; +-&quot;; *p; p++) {
        s[2*k+1] = *p;
        search(s, k+1);
    }
}

void
main(void)
{
    FILE *fin;
    int i;
    char str[30];

    fin = fopen(&quot;zerosum.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;zerosum.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    fscanf(fin, &quot;%d&quot;, &amp;n);

    strcpy(str, &quot;1 2 3 4 5 6 7 8 9&quot;);
    str[2*n-1] = '&#92;&#48;';    /* chop string to only have first n numbers */

    search(str, 0);

    exit(0);
}
[/c]
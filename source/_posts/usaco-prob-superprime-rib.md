title: USACO - PROB SuperPrime Rib
tags:
  - USACO
id: 1711
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-17 10:30:38
---

我的：<!--more-->

[c]
/*
ID: lmy0525
PROG: sprime
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;vector&gt;

using namespace std;

vector &lt;int&gt; sprime[9];

bool isprime(int n)
{
    if (n &lt;= 1)
        return false;
    if (n == 2)
        return true;
    if (n % 2 == 0)
        return false;
    for (int i = 3; i*i &lt;= n; i += 2)
    {
        if (n % i == 0) return false;
    }
    return true;
}

void calc(int n)
{
    for (int i = 0; i &lt; sprime[n-1].size(); i++)
    {
        for (int j = 1; j &lt;= 9; j += 2)
        {
            int num = sprime[n-1][i]*10 + j;
            if (isprime(num))
            {
                sprime[n].push_back(num);
            }
        }
    }
}

int main() {
    ofstream fout (&quot;sprime.out&quot;);
    ifstream fin (&quot;sprime.in&quot;);
    int N;
    fin &gt;&gt; N;
    for (int i = 2; i &lt;= 7; i++)
    {
        if (isprime(i)) sprime[1].push_back(i);
    }
    for (int i = 2; i &lt;= N; i++)
    {
        calc(i);
    }
    for (int i = 0; i &lt; sprime[N].size(); i++)
    {
        fout &lt;&lt; sprime[N][i] &lt;&lt; endl;
    }
    return 0;
}
[/c]
因为Super Prime并不多，所以采用构造的方法。

标程：
[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

FILE *fout;

int
isprime(int n)
{
	int i;

	if(n == 2)
		return 1;

	if(n%2 == 0)
		return 0;

	for(i=3; i*i &lt;= n; i+=2)
		if(n%i == 0)
			return 0;

	return 1;
}

/* print all sprimes possible by adding ndigit digits to the number n */
void
sprime(int n, int ndigit)
{
	if(ndigit == 0) {
		fprintf(fout, &quot;%d\n&quot;, n);
		return;
	}

	n *= 10;
	if(isprime(n+1))
		sprime(n+1, ndigit-1);
	if(isprime(n+3))
		sprime(n+3, ndigit-1);
	if(isprime(n+7))
		sprime(n+7, ndigit-1);
	if(isprime(n+9))
		sprime(n+9, ndigit-1);
}

void
main(void)
{
	int n;
	FILE *fin;

	fin = fopen(&quot;sprime.in&quot;, &quot;r&quot;);
	assert(fin != NULL);
	fout = fopen(&quot;sprime.out&quot;, &quot;w&quot;);
	assert(fout != NULL);

	fscanf(fin, &quot;%d&quot;, &amp;n);

	sprime(2, n-1);
	sprime(3, n-1);
	sprime(5, n-1);
	sprime(7, n-1);
	exit (0);
}
[/c]
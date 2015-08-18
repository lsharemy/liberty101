title: USACO - PROB Prime Palindromes
tags:
  - USACO
id: 1708
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-16 20:48:14
---

我的：<!--more-->

[c]
/*
ID: lmy0525
PROG: pprime
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
#include &lt;cmath&gt;

using namespace std;

ofstream fout;
ifstream fin;

int a, b;
int cnta = 1, cntb = 1;
int solution[8];

bool isPrime(int num)
{
    int num_sqrt = sqrt(num)+1;
    for (int i = 2; i &lt;= sqrt(num); i++)
        if (num % i == 0) return false;
    return true;
}

void generate(int cur, int digit)
{
    if (cur*2 == digit || cur*2-1 == digit)
    {
        int num = 0;
        for (int i = 0; i &lt; cur-1; i++)
        {
            num = num*10 + solution[i];
        }
        if (cur*2 == digit) num = num*10 + solution[cur-1]; // odd
        for (int i = cur-1; i &gt;= 0; i--)
        {
            num = num*10 + solution[i];
        }
        if (num &gt;= a &amp;&amp; num &lt;= b)
        {
            if (isPrime(num))
            {
                fout &lt;&lt; num &lt;&lt; endl;
            }
        }

        return;
    }

    if (cur == 0)
    {
        for (int i = 1; i &lt;= 9; i += 2)
        {
            solution[cur] = i;
            generate(cur+1, digit);
        }
    }
    else
    {
        for (int i = 0; i &lt;= 9; i++)
        {
            solution[cur] = i;
            generate(cur+1, digit);
        }
    }
}

int main() {
    fout.open(&quot;pprime.out&quot;);
    fin.open(&quot;pprime.in&quot;);
    fin &gt;&gt; a &gt;&gt; b;
    int temp = a;
    while (temp/10) cnta++, temp /= 10;
    temp = b;
    while (temp/10) cntb++, temp /= 10;
    cntb = min(cntb, 8);
    for (int i = cnta; i &lt;= cntb; i++)
    {
        generate(0, i);
    }
    return 0;
}
[/c]

这题竟然有HINTS。。。不过写出来和任何一个标程都不太一样。
标程：

[c]
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;
#include &lt;stdlib.h&gt;

FILE *fout;
long a, b;

int
isprime(long n)
{
    long i;

    if(n == 2)
	return 1;

    if(n%2 == 0)
	return 0;

    for(i=3; i*i &lt;= n; i+=2)
	if(n%i == 0)
	    return 0;

    return 1;
}

void
gen(int i, int isodd)
{
    char buf[30];
    char *p, *q;
    long n;

    sprintf(buf, &quot;%d&quot;, i);

    p = buf+strlen(buf);
    q = p - isodd;

    while(q &gt; buf)
	*p++ = *--q;
    *p = '&#92;&#48;';

    n = atol(buf);
    if(a &lt;= n &amp;&amp; n &lt;= b &amp;&amp; isprime(n))
	fprintf(fout, &quot;%ld\n&quot;, n);
}

void
genoddeven(int lo, int hi)
{
    int i;

    for(i=lo; i&lt;=hi; i++)
        gen(i, 1);

    for(i=lo; i&lt;=hi; i++)
        gen(i, 0);
}

void
generate(void)
{
    genoddeven(1, 9);
    genoddeven(10, 99);
    genoddeven(100, 999);
    genoddeven(1000, 9999);
}

void
main(void)
{
    FILE *fin;

    fin = fopen(&quot;pprime.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;pprime.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    fscanf(fin, &quot;%ld %ld&quot;, &amp;a, &amp;b);

    generate();
    exit (0);
}

[/c]

奇数位的回文数总能被11整除：

[c]
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;
#include &lt;stdlib.h&gt;

FILE *fout;
long a, b;

int
isprime(long n)
{
    long i;

    if(n == 2)
        return 1;

    if(n%2 == 0)
        return 0;

    for(i=3; i*i &lt;= n; i+=2)
        if(n%i == 0)
                return 0;

    return 1;
}

void
gen(int i)
{
    char buf[30];
    char *p, *q;
    long n;

    sprintf(buf, &quot;%d&quot;, i);

    p = buf+strlen(buf);
    q = p - 1;

    while(q &gt; buf)
            *p++ = *--q;
    *p = '&#92;&#48;';

    n = atol(buf);
    if(a &lt;= n &amp;&amp; n &lt;= b &amp;&amp; isprime(n))
        fprintf(fout, &quot;%ld\n&quot;, n);
}

void
generate(void)
{
    int i;
    for (i = 1; i &lt;= 9; i++)
      gen(i);

    if(a &lt;= 11 &amp;&amp; 11 &lt;= b)
      fprintf(fout, &quot;11\n&quot;);

    for (i = 10; i &lt;= 9999; i++)
      gen(i);
}

void
main(void)
{
    FILE *fin;

    fin = fopen(&quot;pprime.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;pprime.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    fscanf(fin, &quot;%ld %ld&quot;, &amp;a, &amp;b);

    generate();
    exit (0);
}
[/c]

更简洁的方法：

[c]
#include &lt;iostream.h&gt;
#include &lt;fstream.h&gt;
#include &lt;stdlib.h&gt;

int primelist[100000];
int nprimes;

int isPrime(int num);
int reverse2(int i, int j);

int compare(const void *p, const void *q) { return *(int *)p-*(int *)q; }

void main (void) {
    ifstream infile(&quot;pprime.in&quot;);
    ofstream outfile(&quot;pprime.out&quot;);
    int i, j, begin, end, num;
    infile&gt;&gt;begin&gt;&gt;end;
    if (begin &lt;= 11 &amp;&amp; 11 &lt;=end)
        primelist[nprimes++] = 11;
    for (j = 0; j &lt;= 999; j++)
        for (i = 0; i &lt;= 9; i++)  {
	    num = reverse2(j,i);
	    if (num &gt;= begin &amp;&amp; num &lt;=end &amp;&amp; isPrime(num))
  	        primelist[nprimes++] = num;
        }
    qsort(primelist, nprimes, sizeof(int), compare);
    for (i = 0; i &lt; nprimes; i++)
	outfile &lt;&lt; primelist[i] &lt;&lt; &quot;\n&quot;;
}

int
reverse2(int num, int middle) {
    int i, save=num, digit, combino = 1;
    for (i = 0; num; num /= 10) {
	digit = num % 10;
	i = 10 * i + digit;
	combino *= 10;
    }
    return i+10*combino*save+combino*middle;
}

int isPrime(int num) {
    int i;
    if (num &lt;= 3) return 1;
    if (num%2 == 0 || num%3 ==0) return 0;
    for (i = 5; i*i &lt;= num; i++)
	if (num %i ==0)
	    return 0;
    return 1;
}
[/c]

另一种方法：

[c]
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;math.h&gt;

FILE *f;
int a, b;

int isPrime(int num);
void genPalind(int num, int add, int mulleft, int mulright);
void tryPalind(int num);

int main(){
  int i;
  char first;
  f=fopen(&quot;pprime.in&quot;, &quot;r&quot;);
  fscanf(f, &quot;%d%d&quot;, &amp;a, &amp;b);
  fclose(f);
  f=fopen(&quot;pprime.out&quot;, &quot;w&quot;);
  if (a&lt;=5)
    fprintf(f, &quot;%i\n&quot;, 5);
  if (a&lt;=7 &amp;&amp; b&gt;=7)
    fprintf(f, &quot;%i\n&quot;, 7);
  if (a&lt;=11 &amp;&amp; b&gt;=11)
    fprintf(f, &quot;%i\n&quot;, 11);
  genPalind(3, 0, 100, 1);
  genPalind(5, 0, 10000, 1);
  genPalind(7, 0, 1000000, 1);
  fclose(f);
}

void tryPalind(int num){
  if (!(num&amp;1))
    return;
  if (num&lt;a || num&gt;b)
    return;
  if (!(num%3) || !(num%5) || !(num%7))
    return;
  if (!isPrime(num))
    return;
  fprintf(f, &quot;%d\n&quot;, num);
}

void genPalind(int num, int add, int mulleft, int mulright){
  int i, nmulleft, nmulright;
  if (num==2){
    for (i=0; i&lt;10; i++)
      tryPalind(add+mulleft*i+mulright*i);
  }
  else if (num==1){
    for (i=0; i&lt;10; i++)
      tryPalind(add+mulright*i);
  }
  else {
    nmulleft=mulleft/10;
    nmulright=mulright*10;
    num-=2;
    for (i=0; i&lt;10; i++)
      genPalind(num, add+i*mulleft+i*mulright, nmulleft, nmulright);
  }
}

int isPrime(int num){
  int koren, i;
  koren=(int)sqrt(1.0*num);
  for (i=11; i&lt;=koren; i+=2)
    if (!(num%i))
      return 0;
  return 1;
}
[/c]
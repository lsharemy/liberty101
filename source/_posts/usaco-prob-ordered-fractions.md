title: USACO - PROB Ordered Fractions
tags:
  - USACO
id: 1731
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-18 14:47:35
---

我的1：<!--more-->
[c]
/*
ID: lmy0525
PROG: frac1
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

void solve (int num1, int den1, int num2, int den2, int n)
{
    if (den1+den2 &gt; n) return;

    int num_mid = num1 + num2;
    int den_mid = den1 + den2;

    solve(num1, den1, num_mid, den_mid, n);
    fout &lt;&lt; num_mid &lt;&lt; &quot;/&quot; &lt;&lt; den_mid &lt;&lt; endl;
    solve(num_mid, den_mid, num2, den2, n);
}

int main() {
    fout.open(&quot;frac1.out&quot;);
    fin.open(&quot;frac1.in&quot;);
    int N;
    fin &gt;&gt; N;
    fout &lt;&lt; &quot;0/1&quot; &lt;&lt; endl;
    solve (0, 1, 1, 1, N);
    fout &lt;&lt; &quot;1/1&quot; &lt;&lt; endl;
    return 0;
}
[/c]

额，这个是看了题解的思路，然后写的

我的2：
[c]
/*
ID: lmy0525
PROG: frac1
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
#include &lt;cmath&gt;
#include &lt;vector&gt;

using namespace std;

ofstream fout;
ifstream fin;

typedef struct fraction
{
    int num;
    int den;
}fraction;

vector &lt;fraction&gt; frac;

template &lt;class T&gt;
inline T gcd(T a, T b)
{
    if(a &lt; 0) return gcd(-a, b);
    if(b &lt; 0) return gcd(a, -b);
    return (b==0)?a:gcd(b, a%b);
}

bool fraccompare (fraction a, fraction b)
{
    return a.num*b.den - a.den*b.num &lt; 0;
}

int main() {
    fout.open(&quot;frac1.out&quot;);
    fin.open(&quot;frac1.in&quot;);
    int N;
    fin &gt;&gt; N;
    fraction a;
    a.num = 0;
    a.den = 1;
    frac.push_back(a);
    a.num = 1;
    a.den = 1;
    frac.push_back(a);
    for (int i = 2; i &lt;= N; i++)
    {
        for (int j = 1; j &lt;= i-1; j++)
        {
            if (gcd(i, j) == 1)
            {
                a.num = j;
                a.den = i;
                frac.push_back(a);
            }
        }
    }
    sort (frac.begin(), frac.end(), fraccompare); 
    for (int i = 0; i &lt; frac.size(); i++)
    {
        fout &lt;&lt; frac[i].num &lt;&lt; &quot;/&quot; &lt;&lt; frac[i].den &lt;&lt; endl;
    }
    return 0;
}
[/c]

这个是不满自己不思考就看题解，于是自己想了一种方法再次实现，以减少自己的罪恶感，这个是最直接的方法，生成所有真分数，然后排序。

标程1：
[c]
#include &lt;fstream.h&gt;
#include &lt;stdlib.h&gt;

struct fraction {
    int numerator;
    int denominator;
};

bool rprime(int a, int b){
   int r = a % b;
   while(r != 0){
       a = b;
       b = r;
       r = a % b;
   }
   return(b == 1);
}

int fraccompare (struct fraction *p, struct fraction *q) {
   return p-&gt;numerator * q-&gt;denominator - p-&gt;denominator *q-&gt;numerator;
}

int main(){
   int found = 0;
   struct fraction fract[25600];

   ifstream filein(&quot;frac1.in&quot;);
   int n;
   filein &gt;&gt; n;
   filein.close();

   for(int bot = 1; bot &lt;= n; ++bot){
       for(int top = 0; top &lt;= bot; ++top){
           if(rprime(top,bot)){
               fract[found].numerator = top;
               fract[found++].denominator = bot;
           }
       }
   }

   qsort(fract, found, sizeof (struct fraction), fraccompare);

   ofstream fileout(&quot;frac1.out&quot;);
   for(int i = 0; i &lt; found; ++i)
       fileout &lt;&lt; fract[i].numerator &lt;&lt; '/' &lt;&lt; fract[i].denominator &lt;&lt; endl;
   fileout.close();

   exit (0);
}
[/c]

标程2：
[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

int n;
FILE *fout;

/* print the fractions of denominator &lt;= n between n1/d1 and n2/d2 */
void
genfrac(int n1, int d1, int n2, int d2)
{
	if(d1+d2 &gt; n)	/* cut off recursion */
		return;

	genfrac(n1,d1, n1+n2,d1+d2);
	fprintf(fout, &quot;%d/%d\n&quot;, n1+n2, d1+d2);
	genfrac(n1+n2,d1+d2, n2,d2);
}

void
main(void)
{
	FILE *fin;

	fin = fopen(&quot;frac1.in&quot;, &quot;r&quot;);
	fout = fopen(&quot;frac1.out&quot;, &quot;w&quot;);
	assert(fin != NULL &amp;&amp; fout != NULL);

	fscanf(fin, &quot;%d&quot;, &amp;n);

	fprintf(fout, &quot;0/1\n&quot;);
	genfrac(0,1, 1,1);
	fprintf(fout, &quot;1/1\n&quot;);
}
[/c]
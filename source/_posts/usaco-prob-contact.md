title: USACO - PROB Contact
tags:
  - USACO
id: 1801
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-22 21:46:15
---

暴力题额，用了map。

代码：<!--more-->
[c]
/*
ID: lmy0525
PROG: contact
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
#include &lt;queue&gt;
#include &lt;map&gt;
using namespace std;

ofstream fout;
ifstream fin;
string s;
vector &lt; string &gt; fre[200001];

int main()
{
    fout.open(&quot;contact.out&quot;);
    fin.open(&quot;contact.in&quot;);
    int A, B, N;
    fin &gt;&gt; A &gt;&gt; B &gt;&gt; N;
    string temp;
    while(fin &gt;&gt; temp)
        s += temp;
    for (int i = A; i &lt;= B; i++)
    {
        map &lt;string, int&gt; m;
        map &lt;string, int&gt;::iterator it;
        if (s.size() &lt; i) continue;
        for (int j = 0; j &lt;= s.size()-i; j++)
        {
            m[s.substr(j, i)]++;
        }
        for ( it=m.begin() ; it != m.end(); it++ )
        {
            temp = (*it).first;
            int cnt = (*it).second;
            if (cnt &gt; 0) fre[cnt].push_back(temp);
        }
    }
    int cnt = 0;
    for (int i = 200000; i &gt; 0; i--)
    {
        if (fre[i].size())
        {
            fout &lt;&lt; i &lt;&lt; endl;
            int j;
            for (j = 0; j &lt; fre[i].size(); j++)
            {
                if (j % 6 != 0) fout &lt;&lt; &quot; &quot;;
                fout &lt;&lt; fre[i][j];
                if (j % 6 == 5) fout &lt;&lt; endl;
            }
            if (j % 6 != 0) fout &lt;&lt; endl;
            cnt++;
            if (cnt == N) break;
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

#define MAXBITS 12
#define MAXSEQ (1&lt;&lt;(MAXBITS+1))

typedef struct Seq Seq;
struct Seq {
    unsigned bits;
    int count;
};

Seq seq[MAXSEQ];

/* increment the count for the n-bit sequence &quot;bits&quot; */
void
addseq(unsigned bits, int n)
{
    bits &amp;= (1&lt;&lt;n)-1;
    bits |= 1&lt;&lt;n;
    assert(seq[bits].bits == bits);
    seq[bits].count++;
}

/* print the bit sequence, decoding the 1&lt;&lt;n stuff */
/* recurse to print the bits most significant bit first */
void
printbits(FILE *fout, unsigned bits)
{
    assert(bits &gt;= 1);
    if(bits == 1)	/* zero-bit sequence */
	return;

    printbits(fout, bits&gt;&gt;1);
    fprintf(fout, &quot;%d&quot;, bits&amp;1);
}

int
seqcmp(const void *va, const void *vb)
{
    Seq *a, *b;

    a = (Seq*)va;
    b = (Seq*)vb;

    /* big counts first */
    if(a-&gt;count &lt; b-&gt;count)
	return 1;
    if(a-&gt;count &gt; b-&gt;count)
	return -1;

    /* same count: small numbers first */
    if(a-&gt;bits &lt; b-&gt;bits)
	return -1;
    if(a-&gt;bits &gt; b-&gt;bits)
	return 1;

    return 0;
}

void
main(void)
{
    FILE *fin, *fout;
    int i, a, b, n, nbit, c, j, k;
    unsigned bit;
    char *sep;

    fin = fopen(&quot;contact.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;contact.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    nbit = 0;
    bit = 0;

    for(i=0; i&lt;=MAXBITS; i++)
	for(j=0; j&lt;(1&lt;&lt;i); j++)
	    seq[(1&lt;&lt;i) | j].bits = (1&lt;&lt;i) | j;

    fscanf(fin, &quot;%d %d %d&quot;, &amp;a, &amp;b, &amp;n);

    while((c = getc(fin)) != EOF) {
	if(c != '0' &amp;&amp; c != '1')
	    continue;

	bit &lt;&lt;= 1;
	if(c == '1')
	    bit |= 1;

	if(nbit &lt; b)
	    nbit++;

	for(i=a; i&lt;=nbit; i++)
	    addseq(bit, i);
    }

    qsort(seq, MAXSEQ, sizeof(Seq), seqcmp);

    /* print top n frequencies for number of bits between a and b */
    j = 0;
    for(i=0; i&lt;n &amp;&amp; j &lt; MAXSEQ; i++) {
	if(seq[j].count == 0)
	    break;

	c = seq[j].count;
	fprintf(fout, &quot;%d\n&quot;, c);

	/* print all entries with frequency c */
	sep = &quot;&quot;;
	for(k=0; seq[j].count == c; j++, k++) {
	    fprintf(fout, sep);
	    printbits(fout, seq[j].bits);
	    if(k%6 == 5)
		sep = &quot;\n&quot;;
	    else
		sep = &quot; &quot;;
	}
	fprintf(fout, &quot;\n&quot;);
    }

    exit(0);
}
[/c]
title: USACO - PROB Preface Numbering
tags:
  - USACO
id: 1750
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-19 17:16:45
---

我的：<!--more-->

[c]
/*
ID: lmy0525
PROG: preface
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

char c[7] = {'I', 'V', 'X', 'L', 'C', 'D', 'M'};
int cnt[7]; // I V X L C D M

void calc(int n)
{
    // M MM MMM
    cnt[6] += n / 1000;
    n %= 1000;

    if ( n &gt;= 900 ) // CM
    {
        cnt[6]++;
        cnt[4]++;
        n -= 900;
    }
    else if (n &gt;= 500) // D DC DCC DCCC
    {
        cnt[5]++;
        cnt[4] += (n-500) / 100;
        n = (n-500) % 100;
    }
    else if (n &gt;= 400) // CD
    {
        cnt[4]++;
        cnt[5]++;
        n -= 400;
    }
    else if (n &gt;= 100) // C CC CCC
    {
        cnt[4] += n / 100;
        n %= 100;
    }

    if ( n &gt;= 90 ) // XC
    {
        cnt[4]++;
        cnt[2]++;
        n -= 90;
    }
    else if (n &gt;= 50) // L LX LXX LXXX
    {
        cnt[3]++;
        cnt[2] += (n-50) / 10;
        n = (n-50) % 10;
    }
    else if (n &gt;= 40) // XL
    {
        cnt[2]++;
        cnt[3]++;
        n -= 40;
    }
    else if (n &gt;= 10) // X XX XXX
    {
        cnt[2] += n / 10;
        n %= 10;
    }

    if ( n &gt;= 9 ) // IX 
    {
        cnt[2]++;
        cnt[0]++;
    }
    else if (n &gt;= 5) // V VI VII VIII
    {
        cnt[1]++;
        cnt[0] += n-5;
    }
    else if (n &gt;= 4) // IV
    {
        cnt[0]++;
        cnt[1]++;
    }
    else if (n &gt;= 1) // I II III 
    {
        cnt[0] += n;
    }
}

int main() {
    fout.open(&quot;preface.out&quot;);
    fin.open(&quot;preface.in&quot;);
    int N;
    fin &gt;&gt; N;
    for (int i = 1; i &lt;= N; i++)
    {
        calc(i);
    }
    for (int i = 0; i &lt; 7; i++)
    {
        if (cnt[i])
        {
            fout &lt;&lt; c[i] &lt;&lt; &quot; &quot; &lt;&lt; cnt[i] &lt;&lt; endl;
        }
    }
    return 0;
}
[/c]

标程1：
[c]
/*
PROG: preface
ID: rsc001
*/

#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

static char *encode[] = {
	&quot;&quot;, &quot;I&quot;, &quot;II&quot;, &quot;III&quot;, &quot;IV&quot;,
	&quot;V&quot;, &quot;VI&quot;, &quot;VII&quot;, &quot;VIII&quot;, &quot;IX&quot;,
};

char*
romandigit(int d, char *ivx)
{
	char *s, *p;
	static char str[10];

	for(s=encode[d%10], p=str; *s; s++, p++) {
		switch(*s){
		case 'I':
			*p = ivx[0];
			break;
		case 'V':
			*p = ivx[1];
			break;
		case 'X':
			*p = ivx[2];
			break;
		}
	}
	*p = '&#92;&#48;';
	return str;
}

char*
roman(int n)
{
	static char buf[20];

	strcpy(buf, &quot;&quot;);
	strcat(buf, romandigit(n/1000, &quot;M&quot;));
	strcat(buf, romandigit(n/100,  &quot;CDM&quot;));
	strcat(buf, romandigit(n/10,   &quot;XLC&quot;));
	strcat(buf, romandigit(n,      &quot;IVX&quot;));
	return buf;
}

void
main(void)
{
	FILE *fin, *fout;
	int i, n;
	char *s;
	int count[256];

	fin = fopen(&quot;preface.in&quot;, &quot;r&quot;);
	fout = fopen(&quot;preface.out&quot;, &quot;w&quot;);
	assert(fin != NULL &amp;&amp; fout != NULL);

	fscanf(fin, &quot;%d&quot;, &amp;n);

	for(s=&quot;IVXLCDM&quot;; *s; s++)
		count[*s] = 0;

	for(i=1; i&lt;=n; i++)
		for(s=roman(i); *s; s++)
			count[*s]++;

	for(s=&quot;IVXLCDM&quot;; *s; s++)
		if(count[*s])
			fprintf(fout, &quot;%c %d\n&quot;, *s, count[*s]);

	exit(0);
}
[/c]

标程2：
[c]
#include &lt;fstream.h&gt;

int     Ig = 0;
int     Vg = 0;
int     Xg = 0;
int     Lg = 0;
int     Cg = 0;
int     Dg = 0;
int     Mg = 0;

inline void 
roman (int x)
{
    int     I = 0;
    int     V = 0;
    int     X = 0;
    int     L = 0;
    int     C = 0;
    int     D = 0;
    int     M = 0;
    for ( ; x &gt;= 1000; ++M, x -= 1000);
    for ( ; x &gt;= 500; ++D, x -= 500);
    for ( ; x &gt;= 100; ++C, x -= 100);
    for ( ; x &gt;= 50; ++L, x -= 50);
    for ( ; x &gt;= 10; ++X, x -= 10);
    for ( ; x &gt;= 5; ++V, x -= 5);
    for ( ; x &gt;= 1; ++I, x -= 1);

    while (D &gt; 0 &amp;&amp; (C / 4) &gt; 0) {
	--D; C -= 4; ++M; ++C;
    }
    while (C &gt;= 4) {
	C -= 4; ++D; ++C;
    }
    while (L &gt; 0 &amp;&amp; (X / 4) &gt; 0) {
	--L; X -= 4; ++C; ++X;
    }
    while (X &gt;= 4) {
	X -= 4; ++L; ++X;
    }
    while (V &gt; 0 &amp;&amp; (I / 4) &gt; 0) {
	--V; I -= 4; ++X; ++I;
    }
    while (I &gt;= 4) {
	I -= 4; ++V; ++I;
    }
    Ig += I;
    Vg += V;
    Xg += X;
    Lg += L;
    Cg += C;
    Dg += D;
    Mg += M;
    return;
}

int 
main ()
{

    int     n;
    ifstream filein (&quot;preface.in&quot;);
    filein &gt;&gt; n;
    filein.close ();

    for (int loop = 1; loop &lt;= n; ++loop) {
	roman (loop);
    }

    ofstream fileout (&quot;preface.out&quot;);
    if (Ig != 0) {
	fileout &lt;&lt; 'I' &lt;&lt; ' ' &lt;&lt; Ig &lt;&lt; endl;
    }
    if (Vg != 0) {
	fileout &lt;&lt; 'V' &lt;&lt; ' ' &lt;&lt; Vg &lt;&lt; endl;
    }
    if (Xg != 0) {
	fileout &lt;&lt; 'X' &lt;&lt; ' ' &lt;&lt; Xg &lt;&lt; endl;
    }
    if (Lg != 0) {
	fileout &lt;&lt; 'L' &lt;&lt; ' ' &lt;&lt; Lg &lt;&lt; endl;
    }
    if (Cg != 0) {
	fileout &lt;&lt; 'C' &lt;&lt; ' ' &lt;&lt; Cg &lt;&lt; endl;
    }
    if (Dg != 0) {
	fileout &lt;&lt; 'D' &lt;&lt; ' ' &lt;&lt; Dg &lt;&lt; endl;
    }
    if (Mg != 0) {
	fileout &lt;&lt; 'M' &lt;&lt; ' ' &lt;&lt; Mg &lt;&lt; endl;
    }
    fileout.close ();

    return (0);
}
[/c]

标程3：
[c]
#include &lt;stdio.h&gt;
int     ns[] =
   {1000, 900, 500, 400, 100, 90, 50,  40, 10,  9,  5,   4, 1};
   //&quot;M&quot; &quot;CM&quot;  &quot;D&quot;  &quot;CD&quot; &quot;C&quot; &quot;XC&quot; &quot;L&quot; &quot;XL&quot; &quot;X&quot; &quot;IX&quot; &quot;V&quot; &quot;IV&quot; &quot;I&quot;
char    rs[] = {&quot;IVXLCDM&quot;};
int     match1[] = {6, 4, 5, 4, 4, 2, 3, 2, 2, 0, 1, 0, 0};
int     match2[] = {-1, 6, -1, 5, -1, 4, -1, 3, -1, 2, -1, 1, -1};
int     n;
int     counts[7];

void 
count (int num)
{
    int     sct[] = {3, 1, 1, 1, 3, 1, 1, 1, 3, 1, 1, 1, 3};
    int     i, j = 0;
    for (i = 0; i &lt; 13; i++) {
	while (sct[i] &gt; 0) {
	    if ((num - ns[i]) &gt;= 0) {
		num -= ns[i];
		counts[match1[i]]++;
		if (match2[i] &gt;= 0)
		    counts[match2[i]]++;
		sct[i]--;
	    }
	    else
		break;
	}
    }
}

void 
main ()
{
    FILE   *fp = fopen (&quot;preface.in&quot;, &quot;r&quot;);
    FILE   *wfp = fopen (&quot;preface.out&quot;, &quot;w&quot;);
    int     i;
    fscanf (fp, &quot;%d&quot;, &amp;n);
    for (i = 0; i &lt; 7; i++)
	counts[i] = 0;
    for (i = 1; i &lt;= n; i++)
	count (i);
    for (i = 0; i &lt; 7; i++) {
	if (counts[i])
	    fprintf (wfp, &quot;%c %d\n&quot;, rs[i], counts[i]);
    }
    fclose (fp);
    fclose (wfp);
}
[/c]

标程4：
[c]
#include &lt;fstream&gt;
using namespace std;
int count[7];
int mult[6] = {5, 2, 5, 2, 5, 2}; // The factors between consecutive roman
				  // numeral letter values.
char roman[] = &quot;IVXLCDM&quot;;
int vals[7] = {1, 5, 10, 50, 100, 500, 1000};

int main() {
    ofstream fout (&quot;preface.out&quot;);
    ifstream fin (&quot;preface.in&quot;);
    int n;
    fin &gt;&gt; n;

    for (int i = 1; i &lt;= n; i++) {
	for (int j = 0, temp = i; temp != 0; j++) {
             // If there are more than three of the current letter.
	    if (temp % mult[j] &gt; 3) {
		count[j]++;
                // Checks if it can have a two letter difference
		// (ie. IX instead of IV).
	        if (temp / mult[j] &gt; 0 &amp;&amp; i % vals[j + 2] &gt; vals[j + 1]) {
	            count[j + 2]++;
     	            temp -= mult[j];
                } else
		    count[j + 1]++;
	    } else
		count[j] += temp % mult[j];
	    temp /= mult[j];
	}
    }
    for (int i = 0; i &lt; 7; i++)
     if (count[i])
         fout &lt;&lt; roman[i] &lt;&lt; &quot; &quot; &lt;&lt; count[i] &lt;&lt; endl;
    return 0;
}
[/c]
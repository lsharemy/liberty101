title: USACO - PROB Runaround Numbers
tags:
  - USACO
id: 1759
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-20 11:23:28
---

我的：<!--more-->
[c]
/*
ID: lmy0525
PROG: runround
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
#include &lt;cmath&gt;
#include &lt;vector&gt;
#include &lt;cstring&gt;

using namespace std;

ofstream fout;
ifstream fin;

int M;
char num[10];
bool used[10];
bool done;

bool isRun(char* n)
{
    char temp[10];
    strcpy(temp, n);
    int len = strlen(n);

    int i = 0;
    while(temp[i] != '*')
    {
        temp[i] = '*';
        i = (i + n[i] - '0') % len;
    }

    if (i != 0) return false;

    for (int i = 0; i &lt; len; i++)
    {
        if(temp[i] != '*') return false;
    }

    return true;
}

void generate(int cur, int n)
{
    if (done) return;

    if (cur == n)
    {
        // check
        num[cur] = '&#92;&#48;';
        int temp = atoi(num);
        if (temp &gt; M &amp;&amp; isRun(num))
        {
            fout &lt;&lt; temp &lt;&lt; endl;
            done = true;
        }
    }

    for (int i = 1; i &lt; 10; i++)
    {
        if (!used[i])
        {
            used[i] = true;

            num[cur] = i+'0';
            generate(cur+1, n);

            used[i] = false;
        }
    }
}

int main() {
    fout.open(&quot;runround.out&quot;);
    fin.open(&quot;runround.in&quot;);
    fin &gt;&gt; M;
    for (int i = 1; i &lt; 10; i++) // 1-9
    {
        generate(0, i);
    }
    return 0;
}
[/c]

标程1：
[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

int m;
FILE *fout;

/* check if s is a runaround number;  mark where we've been by writing 'X' */
int
isrunaround(char *s)
{
    int oi, i, j, len;
    char t[10];

    strcpy(t, s);
    len = strlen(t);

    i=0;
    while(t[i] != 'X') {
	oi = i;
	i = (i + t[i]-'0') % len;
	t[oi] = 'X';
    }

    /* if the string is all X's and we ended at 0, it's a runaround */
    if(i != 0)
	return 0;

    for(j=0; j&lt;len; j++)
	if(t[j] != 'X')
	    return 0;

    return 1;
}

/*
 * create an md-digit number in the string s.
 * the used array keeps track of which digits are already taken.
 * s already has nd digits.
 */
void
permutation(char *s, int *used, int nd, int md)
{
    int i;

    if(nd == md) {
	s[nd] = '&#92;&#48;';
	if(atoi(s) &gt; m &amp;&amp; isrunaround(s)) {
	    fprintf(fout, &quot;%s\n&quot;, s);
	    exit(0);
	}
	return;
    }

    for(i=1; i&lt;=9; i++) {
	if(!used[i]) {
	    s[nd] = i+'0';
	    used[i] = 1;
	    permutation(s, used, nd+1, md);
	    used[i] = 0;
	}
    }
}

void
main(void)
{
    FILE *fin;
    char s[10];
    int i, used[10];

    fin = fopen(&quot;runround.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;runround.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    fscanf(fin, &quot;%d&quot;, &amp;m);

    for(i=0; i&lt;10; i++)
	used[i] = 0;

    for(i=1; i&lt;=9; i++)
	permutation(s, used, 0, i);

    assert(0);	/* not reached */
}
[/c]

标程2：
[c]
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;vector&gt;
#include &lt;algorithm&gt;

#define INPUT_FILE &quot;runround.in&quot;
#define OUTPUT_FILE &quot;runround.out&quot;

using namespace std;

void NextNumber(std::vector&lt;int&gt;&amp; number, int Digits) {
    number[Digits - 1]++;
    for (int i = Digits - 1; i &gt;= 0; i--) {
        if (number[i] == 10) {
            number[i] = 1;
            if (i == 0) {
                number.insert (number.begin(),1);
                return;
            } else 
                number[i - 1]++;
        }
    }
    return;
}

bool CheckElement(std::vector&lt;int&gt;::iterator first,
    std::vector&lt;int&gt;::iterator last, int val) {
    while (first &lt; last) {
        if (*first == val) 
            return true;
        ++first;
    }
    return false;
}

void NextUniqueNumber(std::vector&lt;int&gt;&amp; number) {
    std::vector&lt;int&gt; old = number;
    for (int i = 1; i &lt; number.size(); ++i) {
        if (number[i] == 0) number[i]++;
        while (CheckElement (number.begin(),number.begin() + i,number[i])) {
            number[i]++;
            if (number[i] == 10) {
                number[i] = 1;
                NextNumber (number,i);
                i = 1;
                continue;
            }
        }
    }
    return;
}

bool IsRoundNumber(std::vector&lt;int&gt;&amp; number) {
    std::vector&lt;bool&gt; used(10,false);
    for (int i = 0, pos = 0, val = number[0]; i &lt; number.size(); i++) {
        pos = (pos + val) % number.size();
        val = number[pos];
        if (used[pos] == true) return false;
        used[pos] = true;
    }
    return true;
}

unsigned int NextRoundNumber(unsigned int number) {
    std::vector&lt;int&gt; digits;
    for (int i = 0, tens = 1; i &lt;= 10; ++i, tens *= 10) {
        int partial = number / tens;
        if (partial == 0) break;
        partial %= 10;
        digits.push_back(partial);
    }
    std::reverse (digits.begin(),digits.end());
    NextNumber (digits,digits.size());
    NextUniqueNumber (digits);
    while (!IsRoundNumber(digits)) {
        NextNumber (digits,digits.size());
        NextUniqueNumber (digits);
    }
    number = 0;
    for (int i = 0; i &lt; digits.size(); i++) 
        number = 10 * number + digits[i];
    return number;
}

int main(int argc, char *argv[]) {
    ifstream FileInput (INPUT_FILE);
    ofstream FileOutput (OUTPUT_FILE);
    unsigned int Number;
    FileInput &gt;&gt; Number;
    FileOutput &lt;&lt; NextRoundNumber(Number) &lt;&lt; &quot;\n&quot;;
    return 0;
}
[/c]
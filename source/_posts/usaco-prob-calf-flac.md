title: USACO - PROB Calf Flac
tags:
  - USACO
id: 1661
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-13 18:42:57
---

刚开始超时了一次，因为在搜索某个位置的最长回文时，每次都进行了一次substr操作，导致超时，最后改成了找到了最长回文时，才进行一次substr。<!--more-->

[c]
/*
ID: lmy0525
PROG: calfflac
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;

using namespace std;

string s;
int len = 0;
string str = &quot;&quot;;

void calc()
{
    for (int pos = 0; pos &lt; s.size(); pos++)
    {
        if (isalpha(s[pos]))
        {
            int length = 1;
            int l=pos, r=pos;
            for (int left = pos-1, right = pos+1; (right-left+1)&lt;=2000 &amp;&amp; left&gt;=0 &amp;&amp; right &lt; s.size(); left--, right++)
            {
                while( left &gt;= 0 &amp;&amp; !isalpha(s[left]) ) left--;
                while( right &lt; s.size() &amp;&amp; !isalpha(s[right]) ) right++;
                if ( left == -1 || right == s.size() ) break;
                if ( toupper(s[left]) == toupper(s[right]) )
                {
                    l = left, r = right, length += 2;
                }
                else
                {
                    break;
                }
            }
            if (length &gt; len)
            {
                len = length;
                str = s.substr(l, r-l+1);
            }
            length = 0;
            for (int left = pos, right = pos+1; (right-left+1)&lt;=2000 &amp;&amp; left&gt;=0 &amp;&amp; right &lt; s.size(); left--, right++)
            {
                while( left &gt;= 0 &amp;&amp; !isalpha(s[left]) ) left--;
                while( right &lt; s.size() &amp;&amp; !isalpha(s[right]) ) right++;
                if ( left == -1 || right == s.size() ) break;
                if ( toupper(s[left]) == toupper(s[right]) )
                {
                    l = left, r = right, length += 2;
                }
                else
                {
                    break;
                }
            }
            if (length &gt; len)
            {
                len = length;
                str = s.substr(l, r-l+1);
            }
        }
    }
}

int main()
{
    ofstream fout (&quot;calfflac.out&quot;);
    ifstream fin (&quot;calfflac.in&quot;);
    string temp;
    while(getline(fin, temp))
    {
        s += temp+&quot;\n&quot;;
    }
    calc();
    fout &lt;&lt; len &lt;&lt; endl;
    fout &lt;&lt; str &lt;&lt; endl;
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

char fulltext[21000];
char text[21000];

char *pal;
int pallen;

void
findpal(void)
{
    char *p, *fwd, *bkwd, *etext;
    int len;

    etext = text+strlen(text);
    for(p=text; *p; p++) {
	/* try palindrome with *p as center character */
	for(fwd=bkwd=p; bkwd &gt;= text &amp;&amp; fwd &lt; etext &amp;&amp; *fwd == *bkwd;
				fwd++, bkwd--)
			;
	bkwd++;
	len = fwd - bkwd;
	if(len &gt; pallen) {
	    pal = bkwd;
	    pallen = len;
	}

	/* try palindrome with *p as left middle character */
	for(bkwd=p, fwd=p+1;
	          bkwd &gt;= text &amp;&amp; fwd &lt; etext &amp;&amp; *fwd == *bkwd; fwd++, bkwd--)
			;
	bkwd++;
	len = fwd - bkwd;
	if(len &gt; pallen) {
	    pal = bkwd;
	    pallen = len;
	}
    }
}

void
main(void)
{
    FILE *fin, *fout;
    char *p, *q;
    int c, i, n;

    fin = fopen(&quot;calfflac.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;calfflac.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    /* fill fulltext with input, text with just the letters */
    p=fulltext;
    q=text;
    while((c = getc(fin)) != EOF) {
	if(isalpha(c))
	    *q++ = tolower(c);
	*p++ = c;
    }
    *p = '&#92;&#48;';
    *q = '&#92;&#48;';

    findpal();

    fprintf(fout, &quot;%d\n&quot;, pallen);

    /* find the string we found in the original text
       by finding the nth character */
	n = pal - text;
    for(i=0, p=fulltext; *p; p++)
	if(isalpha(*p))
	    if(i++ == n)
		break;
    assert(*p != '&#92;&#48;');

    /* print out the next pallen characters */
    for(i=0; i&lt;pallen &amp;&amp; *p; p++) {
	fputc(*p, fout);
	if(isalpha(*p))
	    i++;
    }
    fprintf(fout, &quot;\n&quot;);

    exit(0);
}
[/c]

标程在一个只包含字母的串里找最长回文，然后在原串中找到这个回文串对应的串。
title: USACO - PROB Your Ride Is Here
tags:
  - USACO
id: 1584
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-11 15:44:45
---

我的：<!--more-->

[cpp]
/*
ID: lmy0525
LANG: C++
PROG: ride
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;

using namespace std;

int main()
{
    ofstream fout (&quot;ride.out&quot;);
    ifstream fin (&quot;ride.in&quot;);
    string comet, group;
    fin &gt;&gt; comet &gt;&gt; group;
    int comet_num = 1, group_num = 1;
    for (int i = 0; i &lt; comet.size(); i++)
    {
        comet_num *= (comet[i] - 'A' + 1);
    }
    for (int i = 0; i &lt; group.size(); i++)
    {
        group_num *= (group[i] - 'A' + 1);
    }
    if ( comet_num%47 == group_num%47 )
        fout &lt;&lt; &quot;GO&quot; &lt;&lt; endl;
    else
        fout &lt;&lt; &quot;STAY&quot; &lt;&lt; endl;
    return 0;
}
[/cpp]

标程：

[c]
#include &lt;stdio.h&gt;
#include &lt;ctype.h&gt;

int
hash(char *s)
{
	int i, h;

	h = 1;
	for(i=0; s[i] &amp;&amp; isalpha(s[i]); i++)
		h = ((s[i]-'A'+1)*h) % 47;
	return h;
}

void
main(void)
{
	FILE *in, *out;
	char comet[100], group[100];  /* bigger than necessary, room for newline */

	in = fopen(&quot;input.txt&quot;, &quot;r&quot;);
	out = fopen(&quot;output.txt&quot;, &quot;w&quot;);

	fgets(comet, sizeof comet, in);
	fgets(group, sizeof group, in);

	if(hash(comet) == hash(group))
		fprintf(out, &quot;GO\n&quot;);
	else
		fprintf(out, &quot;STAY\n&quot;);
	exit (0);
}
[/c]

即使不是越界，标程还是有防越界，还有就是，通用的过程应该写成函数。

还有，原来这个题意就是一个简单的hash
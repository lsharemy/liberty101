title: USACO - PROB Greedy Gift Givers
tags:
  - USACO
id: 1591
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-11 17:06:37
---

我的：<!--more-->

[c]
/*
ID: lmy0525
LANG: C++
PROG: gift1
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;map&gt;

using namespace std;

int main()
{
    ofstream fout (&quot;gift1.out&quot;);
    ifstream fin (&quot;gift1.in&quot;);
    int NP;
    fin &gt;&gt; NP;
    string name[10]; // 2 - 10
    map &lt;string, int&gt; initial;
    map &lt;string, int&gt; remain;
    map &lt;string, int&gt; receive;
    for (int i = 0; i &lt; NP; i++)
        fin &gt;&gt; name[i];
    for (int i = 0; i &lt; NP; i++)
    {
        string giving;
        int money, num;
        fin &gt;&gt; giving;
        fin &gt;&gt; money &gt;&gt; num;
        initial[giving] = money;
        if (num == 0)
            remain[giving] = money;
        else
            remain[giving] = money % num;
        for (int j = 0; j &lt; num; j++)
        {
            string recipient;
            fin &gt;&gt; recipient;
            receive[recipient] += money / num;
        }
    }
    for (int i = 0; i &lt; NP; i++)
    {
        fout &lt;&lt; name[i] &lt;&lt; &quot; &quot; &lt;&lt; (remain[name[i]] + receive[name[i]]) - initial[name[i]] &lt;&lt;endl;
    }

    return 0;
}
[/c]

标程：

[c]
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

#define MAXPEOPLE 10
#define NAMELEN	32

typedef struct Person Person;
struct Person {
    char name[NAMELEN];
    int total;
};

Person people[MAXPEOPLE];
int npeople;

void
addperson(char *name)
{
    assert(npeople &lt; MAXPEOPLE);
	strcpy(people[npeople].name, name);
    npeople++;
}

Person*
lookup(char *name)
{
    int i;

    /* look for name in people table */
    for(i=0; i&lt;npeople; i++)
	if(strcmp(name, people[i].name) == 0)
	    return &amp;people[i];

    assert(0);	/* should have found name */
}

int
main(void)
{
    char name[NAMELEN];
    FILE *fin, *fout;
    int i, j, np, amt, ng;
    Person *giver, *receiver;

    fin = fopen(&quot;gift1.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;gift1.out&quot;, &quot;w&quot;);

    fscanf(fin, &quot;%d&quot;, &amp;np);
    assert(np &lt;= MAXPEOPLE);

    for(i=0; i&lt;np; i++) {
	fscanf(fin, &quot;%s&quot;, name);
	addperson(name);
    }

    /* process gift lines */
    for(i=0; i&lt;np; i++) {
	fscanf(fin, &quot;%s %d %d&quot;, name, &amp;amt, &amp;ng);
	giver = lookup(name);

	for(j=0; j&lt;ng; j++) {
	    fscanf(fin, &quot;%s&quot;, name);
	    receiver = lookup(name);
	    giver-&gt;total -= amt/ng;
	    receiver-&gt;total += amt/ng;
	}
    }

    /* print gift totals */
    for(i=0; i&lt;np; i++)
	printf(&quot;%s %d\n&quot;, people[i].name, people[i].total);
    exit (0);
}
[/c]

之前也想用标程这种方法的，不过没想到用结构体，同时，也没有想到用函数去写一些通用的例程。
title: USACO - PROB Name That Number
tags:
  - USACO
id: 1619
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-12 20:17:12
---

我的：<!--more-->

[c]
/*
ID: lmy0525
PROG: namenum
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;vector&gt;

using namespace std;

int AtoN[] = {2,2,2,3,3,3,4,4,4,5,5,5,6,6,6,7,7,7,7,8,8,8,9,9,9,9};

int main()
{
    ofstream fout (&quot;namenum.out&quot;);
    ifstream fin (&quot;namenum.in&quot;);
    ifstream findic (&quot;dict.txt&quot;);
    string num;
    fin &gt;&gt; num;
    string s;
    bool flag = false;
    while (findic &gt;&gt; s)
    {
        if (s.size() == num.size())
        {
            string temp = s;
            for (int i = 0; i &lt; s.size(); i++)
            {
                temp[i] = AtoN[(temp[i]-'A')]+'0';
            }
            if (temp == num)
            {
                fout &lt;&lt; s &lt;&lt; endl;
                flag = true;
            }
        }
    }
    if (!flag) fout &lt;&lt; &quot;NONE&quot; &lt;&lt; endl;
    return 0;
}
[/c]

枚举+二分：

[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
char num[12],sol[12];
char dict[5000][13];
int nsolutions = 0;
int nwords;
int maxlen;
FILE *out;

void calc (int charloc, int low, int high) {
    if (charloc == maxlen) {
        sol[charloc] = '&#92;&#48;';
        for (int x = low; x &lt; high; x++) {
            if (strcmp (sol, dict[x]) == 0) {
                fprintf (out, &quot;%s\n&quot;, sol);
                nsolutions++;
            }
        }
        return;
   }
   if (charloc &gt; 0) {
        for (int j=low; j &lt;= high; j++){
            if (sol[charloc-1] == dict[j][charloc-1]) {
                low=j;
                while (sol[charloc-1] == dict[j][charloc-1])
                    j++;
                high=j;
                break;
            }
            if (j == high) return;
        }
    }
    if (low &gt; high) return;
    switch(num[charloc]){
      case '2':sol[charloc] = 'A'; calc(charloc+1,low,high);
               sol[charloc] = 'B'; calc(charloc+1,low,high);
               sol[charloc] = 'C'; calc(charloc+1,low,high);
               break;
      case '3':sol[charloc] = 'D'; calc(charloc+1,low,high);
               sol[charloc] = 'E'; calc(charloc+1,low,high);
               sol[charloc] = 'F'; calc(charloc+1,low,high);
               break;
      case '4':sol[charloc] = 'G'; calc(charloc+1,low,high);
               sol[charloc] = 'H'; calc(charloc+1,low,high);
               sol[charloc] = 'I'; calc(charloc+1,low,high);
               break;
      case '5':sol[charloc] = 'J'; calc(charloc+1,low,high);
               sol[charloc] = 'K'; calc(charloc+1,low,high);
               sol[charloc] = 'L'; calc(charloc+1,low,high);
               break;
      case '6':sol[charloc] = 'M'; calc(charloc+1,low,high);
               sol[charloc] = 'N'; calc(charloc+1,low,high);
               sol[charloc] = 'O'; calc(charloc+1,low,high);
               break;
      case '7':sol[charloc] = 'P'; calc(charloc+1,low,high);
               sol[charloc] = 'R'; calc(charloc+1,low,high);
               sol[charloc] = 'S'; calc(charloc+1,low,high);
               break;
      case '8':sol[charloc] = 'T'; calc(charloc+1,low,high);
               sol[charloc] = 'U'; calc(charloc+1,low,high);
               sol[charloc] = 'V'; calc(charloc+1,low,high);
               break;
      case '9':sol[charloc] = 'W'; calc(charloc+1,low,high);
               sol[charloc] = 'X'; calc(charloc+1,low,high);
               sol[charloc] = 'Y'; calc(charloc+1,low,high);
               break;
   }
}

int main(){
    FILE *in=fopen (&quot;namenum.in&quot;, &quot;r&quot;);
    FILE *in2=fopen (&quot;dict.txt&quot;, &quot;r&quot;);
    int j;
    out=fopen (&quot;namenum.out&quot;,&quot;w&quot;);
    for (nwords = 0; fscanf (in2, &quot;%s&quot;, &amp;dict[nwords++]) != EOF; )
        ;
    fscanf (in, &quot;%s&quot;,&amp;num);
    maxlen = strlen(num);
    calc (0, 0, nwords);
    if (nsolutions == 0) fprintf(out,&quot;NONE\n&quot;);
    return 0;
}
[/c]

这个太BT了，我是写不出来。

另外一个解法：

[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;

int main() {
    FILE *in = fopen (&quot;namenum.in&quot;, &quot;r&quot;);
    FILE *in2 = fopen (&quot;dict.txt&quot;, &quot;r&quot;);
    FILE *out = fopen (&quot;namenum.out&quot;,&quot;w&quot;);
    int nsolutions = 0;
    int numlen;
    char word[80], num[80], *p, *q, map[256];
    int i, j;
    map['A'] = map['B'] = map['C'] = '2';
    map['D'] = map['E'] = map['F'] = '3';
    map['G'] = map['H'] = map['I'] = '4';
    map['J'] = map['K'] = map['L'] = '5';
    map['M'] = map['N'] = map['O'] = '6';
    map['P'] = map['R'] = map['S'] = '7';
    map['T'] = map['U'] = map['V'] = '8';
    map['W'] = map['X'] = map['Y'] = '9';
    fscanf (in, &quot;%s&quot;,num);
    numlen = strlen(num);
    while (fscanf (in2, &quot;%s&quot;, word) != EOF) {
        for (p=word, q=num; *p &amp;&amp; *q; p++, q++) {
            if (map[*p] != *q)
                break;
        }
        if (*p == '&#92;&#48;' &amp;&amp; *q == '&#92;&#48;') {
            fprintf (out, &quot;%s\n&quot;, word);
            nsolutions++;
        }
    }
    if (nsolutions == 0) fprintf(out,&quot;NONE\n&quot;);
    return 0;
}
[/c]

这个思路跟我的是一样的，忽忽。
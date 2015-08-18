title: USACO - PROB Broken Necklace
tags:
  - USACO
id: 1595
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-11 20:47:41
---

我的：<!--more-->

[c]
/*
ID: lmy0525
LANG: C++
PROG: beads
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;

using namespace std;

int main()
{
    ofstream fout (&quot;beads.out&quot;);
    ifstream fin (&quot;beads.in&quot;);
    int N; // 3 - 350
    string necklace;
    string necklace3;
    fin &gt;&gt; N &gt;&gt; necklace;
    necklace3 = necklace + necklace + necklace;
    int res = 0;
    for (int i = N; i &lt; 2*N; i++)
    {
        int cnt1 = 0;
        char color1 = necklace3[i-1];
        for (int j = i-1; j &gt; i-1-N; j--)
        {
            if (necklace3[j] == 'w')
            {
                cnt1++;
            }
            else
            {
                if (color1 == 'w')
                {
                    color1 = necklace3[j];
                    cnt1++;
                }
                else if (color1 == necklace3[j])
                {
                    cnt1++;
                }
                else
                {
                    break;
                }
            }
        }
        char color2 = necklace3[i];
        int cnt2 = 0;
        for (int j = i; j &lt; i+N; j++)
        {
            if (necklace3[j] == 'w')
            {
                cnt2++;
            }
            else
            {
                if (color2 == 'w')
                {
                    color2 = necklace3[j];
                    cnt2++;
                }
                else if (color2 == necklace3[j])
                {
                    cnt2++;
                }
                else
                {
                    break;
                }
            }
        }
        int cnt;
        if (cnt1 + cnt2 &gt; N)
            cnt = N;
        else
            cnt = cnt1+cnt2;
        res = max(cnt, res);
    }
    fout &lt;&lt; res &lt;&lt; endl;
    return 0;
}
[/c]

标程：

[c]
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;
#include &lt;assert.h&gt;

#define MAXN 400

char necklace[MAXN];
int len;

/*
 * Return n mod m.  The C % operator is not enough because
 * its behavior is undefined on negative numbers.
 */
int
mod(int n, int m)
{
    while(n &lt; 0)
	n += m;
    return n%m;
}

/*
 * Calculate number of beads gotten by breaking
 * before character p and going in direction dir,
 * which is 1 for forward and -1 for backward.
 */
int
nbreak(int p, int dir)
{
    char color;
    int i, n;

    color = 'w';

    /* Start at p if going forward, bead before if going backward */
    if(dir &gt; 0)
	i = p;
    else
	i = mod(p-1, len);

    /* We use &quot;n&lt;len&quot; to cut off loops that go around the whole necklace */
    for(n=0; n&lt;len; n++, i=mod(i+dir, len)) {
	/* record which color we're going to collect */
	if(color == 'w' &amp;&amp; necklace[i] != 'w')
	    color = necklace[i];

	/*
	 * If we've chosen a color and see a bead
	 * not white and not that color, stop
	 */
	if(color != 'w' &amp;&amp; necklace[i] != 'w' &amp;&amp; necklace[i] != color)
	    break;
    }
    return n;
}

void
main(void)
{
    FILE *fin, *fout;
    int i, n, m;

    fin = fopen(&quot;beads.in&quot;, &quot;r&quot;);
    fout = fopen(&quot;beads.out&quot;, &quot;w&quot;);
    assert(fin != NULL &amp;&amp; fout != NULL);

    fscanf(fin, &quot;%d %s&quot;, &amp;len, necklace);
    assert(strlen(necklace) == len);

    m = 0;
    for(i=0; i&lt;len; i++) {
	n = nbreak(i, 1) + nbreak(i, -1);
	if(n &gt; m)
	    m = n;
    }

    /*
     * If the whole necklace can be gotten with a good
     * break, we'll sometimes count beads more than
     * once.  this can only happen when the whole necklace
     * can be taken, when beads that can be grabbed from
     * the right of the break can also be grabbed from the left.
     */
    if(m &gt; len)
	m = len;

    fprintf(fout, &quot;%d\n&quot;, m);
    exit (0);
}
[/c]

又没想到怎么用函数来实现计算可以获得的某个方向上的珠子数，刚开始的想法还是跟标程一样的，只是没想到这么实现，傻乎乎的把字符串复制成3份

DP:

[c]
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;
#include &lt;algorithm&gt;

using namespace std;

FILE *in,*out;

int main () {
   in = fopen(&quot;beads.in&quot;, &quot;r&quot;);
   out = fopen (&quot;beads.out&quot;, &quot;w&quot;);

   int n;
   char tmp[400], s[800];
   fscanf(in, &quot;%d %s&quot;, &amp;n, tmp);

   strcpy(s, tmp);
   strcat(s, tmp);

   int left[800][2], right[800][2];
   left[0][0] = left[0][1] = 0;

   for (int i=1; i&lt;= 2 * n; i++){
       if (s[i - 1] == 'r'){
           left[i][0] = left[i - 1][0] + 1;
           left[i][1] = 0;
       } else if (s[i - 1] == 'b'){
           left[i][1] = left[i - 1][1] + 1;
           left[i][0] = 0;
       } else {
           left[i][0] = left[i - 1][0] + 1;
           left[i][1] = left[i - 1][1] + 1;
       }
     }

   right[2 * n][0] = right[2 * n][1] = 0;
   for (int i=2 * n - 1; i &gt;= 0; i--){
       if (s[i] == 'r'){
           right[i][0] = right[i + 1][0] + 1;
           right[i][1] = 0;
       } else if (s[i] == 'b'){
           right[i][1] = right[i + 1][1] + 1;
           right[i][0] = 0;
       } else {
           right[i][0] = right[i + 1][0] + 1;
           right[i][1] = right[i + 1][1] + 1;
       }
   }

   int m = 0;
   for (int i=0; i&lt;2 * n; i++)
       m = max(m, max(left[i][0], left[i][1]) + max(right[i][0], right[i][1]));
   m = min(m, n);
   fprintf(out, &quot;%d\n&quot;, m);
   fclose(in); fclose(out);
   return 0;
}
[/c]

现在至少能看懂DP了，忽忽，这样复杂度变成了O(n)。思想是，先把字符串复制成2份，然后从左向右计算分割点i处向左能得到的红珠数left[i][0]、绿珠数left[i][1]，从右向左计算分割点i处向右能得到的红珠数right[i][0]、绿珠数right[i][1]，最后计算在i分割能得到的珠数max(left[i][0], left[i][1]) + max(right[i][0], right[i][1])，然后取最大值。

简洁：

[c]
/* This solution simply changes the string s into ss, then for every starting
// symbol it checks if it can make a sequence simply by repeatedly checking
// if a sequence can be found that is longer than the current maximum one.
*/

#include &lt;iostream&gt;
#include &lt;fstream&gt;
using namespace std;

int main() {
  fstream input, output;
  string inputFilename = &quot;beads.in&quot;, outputFilename = &quot;beads.out&quot;;
  input.open(inputFilename.c_str(), ios::in);
  output.open(outputFilename.c_str(), ios::out);

  int n, max=0, current, state, i, j;
  string s;
  char c;

  input &gt;&gt; n &gt;&gt; s;
  s = s+s;
  for(i=0; i&lt;n; i++) {
    c = (char) s[i];
    if(c == 'w')
      state = 0;
    else
      state = 1;
    j = i;
    current = 0;
    while(state &lt;= 2) {
      // dont go further in second string than starting position in first string
      while(j&lt;n+i &amp;&amp; (s[j] == c || s[j] == 'w')) {
        current++;
        j++;
      } // while
      state++;
      c = s[j];
    } // while
    if(current &gt; max)
      max = current;
  } // for

  output &lt;&lt; max &lt;&lt; endl;
  return 0;
} // main
[/c]

代码很短，不过还是O(n^2)算法吧。转换了个思想，变成了最长的串，这种串的特性为，前面是一种颜色，后面是另一种颜色。也就是分割点为颜色的分割处。一般都是想到找到分割数，往两边数。这个算法是找到最开始的地方，往后数。
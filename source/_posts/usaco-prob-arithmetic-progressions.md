title: USACO - PROB Arithmetic Progressions
tags:
  - USACO
id: 1691
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-16 15:30:14
---

我的：<!--more-->

[c]
/*
ID: lmy0525
PROG: ariprog
LANG: C++
*/
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
#include &lt;vector&gt;

using namespace std;

int N, M;
int bisquares[63001];
int is_bisquare[125001]={0};
vector &lt; pair&lt;int, int&gt; &gt; res;

bool check(int a, int n)
{
    for (int i = 1; i &lt; N; i++)
    {
        if (!is_bisquare[a+i*n]) return false;
    }
    return true;
}

int main() {
    ofstream fout (&quot;ariprog.out&quot;);
    ifstream fin (&quot;ariprog.in&quot;);
    fin &gt;&gt; N &gt;&gt; M;
    int cnt = 0;
    for (int i = 0; i &lt;= M; i++)
    {
        for (int j = 0; j &lt;= M; j++)
        {
            int temp = i*i+j*j;
            bisquares[cnt++] = temp;
            is_bisquare[temp] = 1;
        }
    }
    sort (bisquares, bisquares+cnt);
    cnt = unique(bisquares, bisquares+cnt) - bisquares;
    for (int i = 0; i &lt; cnt; i++)
    {
        for (int n = 1; bisquares[i]+(N-1)*n &lt;= bisquares[cnt-1]; n++)
        {
            if (check(bisquares[i], n))
            {
                res.push_back(make_pair(n, bisquares[i]));
            }
        }
    }
    sort (res.begin(), res.end());
    if (res.size())
    {
        for (int i = 0; i &lt; res.size(); i++)
        {
            fout &lt;&lt; res[i].second &lt;&lt; &quot; &quot; &lt;&lt; res[i].first &lt;&lt; endl;
        }
    }
    else
    {
        fout &lt;&lt; &quot;NONE&quot; &lt;&lt; endl;
    }
    return 0;
}
[/c]

这次相比两个标程，更喜欢自己的代码，至少看着舒服点，忽忽

标程之一：

[c]
#include &lt;stdio.h&gt;
#include &lt;assert.h&gt;
#include &lt;string&gt;

using namespace std;

// open files
FILE *fin = fopen (&quot;ariprog.in&quot;, &quot;r&quot;);
FILE *fout = fopen (&quot;ariprog.out&quot;, &quot;w&quot;);

// global variables
unsigned int N, M, maxMM;
unsigned int numbers [65000];
unsigned int number_size = 0;
unsigned char num_available [125001];
unsigned char dist_available [125001];
int have_res = 0;
int skipstep = 1;

// read the input

int read_input () {
    fscanf (fin, &quot;%d %d&quot;, &amp;N, &amp;M);
    return 0;
}

int cmp_int (const void *a, const void *b) {
    return (*(int *)a - *(int *)b);
}

void asm_num (int a, int b) {
    for (unsigned int n = 1; n &lt; N; n++)
        if (num_available [a + n * b] == 0) return;

    fprintf (fout, &quot;%d %d\n&quot;, a, b);
    have_res ++;
    if (have_res==1)
        skipstep = b;

}

void asm_num () {
    for (unsigned int b = 1; b &lt; maxMM; b+=skipstep) {
        if (dist_available [b]) {
            for (unsigned int p = 0; p &lt; number_size &amp;&amp; numbers [p] + (N -
1) * b &lt;= maxMM; p++)
                asm_num (numbers [p], b);
        }
    }
}

int process () {
    memset (num_available, 0, sizeof (unsigned char) * 125001);
    memset (dist_available, 0, sizeof (unsigned char) * 125001);

    for (unsigned int m1 = 0; m1 &lt;= M; m1++) {
        for (unsigned int m2 = m1; m2 &lt;= M; m2++) {
            int n = m1 * m1 + m2 * m2;

            if (!num_available [n]) {
                num_available [n] = 1;
                numbers [number_size++] = n;
            }
        }
    }

    qsort (numbers, number_size, sizeof (unsigned int), cmp_int);

    maxMM = M * M + M * M;
    for (unsigned int n1 = 0; n1 &lt; number_size; n1++) {
        unsigned int _n1 = numbers [n1];
        for (unsigned int n2 = n1 + 1; n2 &lt; number_size &amp;&amp; _n1 + (numbers
[n2] - _n1) * (N - 1) &lt;= maxMM; n2++) {
            assert (numbers [n2] - _n1 &gt;= 0 &amp;&amp; numbers [n2] - _n1 &lt; 125001);
            if (num_available [_n1 + (numbers [n2] - _n1) * (N - 1)] &amp;&amp;
                num_available [_n1 + (numbers [n2] - _n1) * (N - 2)])
                dist_available [numbers [n2] - _n1] = true;
        }
    }

    asm_num ();

    if (!have_res) fprintf (fout, &quot;NONE\n&quot;);

    return 0;
}

int main () {
    read_input ();
    process ();
    fclose (fin);
    fclose (fout);
    return 0;
}
[/c]

标程之二：

[c]
#include &lt;fstream&gt;
#include &lt;iostream&gt;

using namespace std;
void quicksort (int[], int ,int);
int pivotlist (int[], int,int);

ofstream out (&quot;ariprog.out&quot;);

int n;
int main () {
    ifstream in (&quot;ariprog.in&quot;);

    bool array[125001] = {false}, noneF;
    int m, upper, upperdef, def, p;
    int places[300000], pl = 0;
    noneF = true;

    in&gt;&gt;n&gt;&gt;m;

    for (int i = 0; i &lt;= m; i++)
        for (int j = 0; j &lt;= m; j++) {
            if (!array[i*i+j*j]) {
                places[pl] = i*i+j*j;   //Saving generated numbers
                pl++;
            }
            array[i*i+j*j] = true;
        }

    upper = 2*m*m;
    upperdef = upper/(n-1);

    quicksort (places, 0, pl-1);

    for ( def = 1; def&lt;=upperdef; def++) // Loop to check for solutions
                                       // It looks for solutions in
                                       // correct order so you
                                       // print the solution directly
                                       // without sorting first, thnx to who said:
                                       // Trade Memory for Speed !!
    {
        for ( p = 0; places[p]&lt;=(upper-((n-2)*def)); p++) {
            bool is;
            is = true;
            int where;

            for (int c = (n-1); c&gt;=0 ; c--)
                    if (!array[places[p]+c*def]) {
                        is = false;
                        where = (p+c*def);
                        break;
                    }

            if (is) {
                noneF = false;
                out&lt;&lt;places[p]&lt;&lt;&quot; &quot;&lt;&lt;def&lt;&lt;endl;
            }
        }
    }

    if (noneF)
        out&lt;&lt;&quot;NONE&quot;&lt;&lt;endl;

    return 0;
}

void quicksort (int array[], int start, int last) {
    int pivot;
    if (start &lt; last) {
        pivot = pivotlist(array, start,last);
        quicksort (array, start,pivot-1);
        quicksort (array, pivot+1,last);
    }
}

int pivotlist(int array[], int f, int l) {
    int pivotpoint;
    int pivotvalue, temp;

    pivotvalue = array[f];
    pivotpoint = f;

    for (int i = f+1;i&lt;=l; i++) {
       	if (array[i]&lt;pivotvalue) {
      	    pivotpoint++;
            temp = array[i];
            array[i] = array[pivotpoint];
            array[pivotpoint] = temp;
   	 }
   }
   temp = array[f];
   array[f] = array[pivotpoint];
   array[pivotpoint] = temp;

   return pivotpoint;
}
[/c]
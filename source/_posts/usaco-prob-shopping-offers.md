title: USACO - PROB Shopping Offers
tags:
  - A
  - USACO
id: 1858
categories:
  - wordpress
  - index.php
  - csit
date: 2012-08-13 16:37:15
---

好吧，DP，看了别人的题解才写出来的

代码：<!--more-->

[c]
/*
ID: lmy0525
PROG: shopping
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
#include &lt;stack&gt;
using namespace std;

ofstream fout;
ifstream fin;

vector &lt; pair&lt;int, int&gt; &gt; offer[100];
int price[6], need[6], code[1000], reduce[100];
int f[6][6][6][6][6];

int main()
{
    fout.open(&quot;shopping.out&quot;);
    fin.open(&quot;shopping.in&quot;);
    int s;
    fin &gt;&gt; s;
    for (int i = 1; i &lt;= s; i++)
    {
        int n;
        fin &gt;&gt; n;
        for (int j = 0; j &lt; n; j++)
        {
            int c, k;
            fin &gt;&gt; c &gt;&gt; k;
            offer[i].push_back(make_pair(c, k));
        }
        fin &gt;&gt; reduce[i];
    }
    int b;
    fin &gt;&gt; b;
    for (int i = 1; i &lt;= b; i++)
    {
        int c, k, p;
        fin &gt;&gt; c &gt;&gt; k &gt;&gt; p;
        code[c] = i;
        need[i] = k;
        price[i] = p;
    }
    for (int i1 = 0; i1 &lt;= need[1]; i1++)
    for (int i2 = 0; i2 &lt;= need[2]; i2++)
    for (int i3 = 0; i3 &lt;= need[3]; i3++)
    for (int i4 = 0; i4 &lt;= need[4]; i4++)
    for (int i5 = 0; i5 &lt;= need[5]; i5++)
    {
        f[i1][i2][i3][i4][i5] = price[1]*i1+price[2]*i2+price[3]*i3+price[4]*i4+price[5]*i5;
        for (int i = 1; i &lt;= s; i++)
        {
            int temp[6] = {0, 0, 0, 0, 0, 0};
            bool flag = true;
            for (int j = 0; j &lt; offer[i].size(); j++)
            {
                int c, k;
                c = offer[i][j].first;
                k = offer[i][j].second;
                if (code[c] == 0)
                {
                    flag = false;
                    break;
                }
                temp[code[c]] = k;
            }
            if (!flag) continue;
            if (!(temp[1]&lt;=i1 &amp;&amp; temp[2]&lt;=i2 &amp;&amp; temp[3]&lt;=i3 &amp;&amp; temp[4]&lt;=i4 &amp;&amp; temp[5]&lt;=i5)) continue;
            f[i1][i2][i3][i4][i5] = min(f[i1][i2][i3][i4][i5], f[i1-temp[1]][i2-temp[2]][i3-temp[3]][i4-temp[4]][i5-temp[5]]+reduce[i]);
        }
    }
    fout &lt;&lt; f[need[1]][need[2]][need[3]][need[4]][need[5]] &lt;&lt; endl;
    return 0;
}
[/c]

标程1（转换成图论@_@）：

[c]
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;

/* maximum number of offers */
/* 100 offers + 5 degenerate offers */
#define MAXO 105

typedef struct OFFER_T
 {
  int nitem; /* number of items in the offer */
  int itemid[5]; /* item's id */
  int itemamt[5]; /* item's amount */
  int cost; /* the cost of this offer */
 } offer_t;

offer_t offers[MAXO];
int noffer;

/* the cost of each basket type */
int cost[7776];

/* the item statistics */
int itemid[5]; /* the id */
int itemcst[5]; /* the cost of buying just 1 */
int nitem;

/* heap used by Dijkstra's algorithm */
int heap[7776];
int hsize;
int hloc[7776]; /* location of baskets within the heap */

/* debugging routine */
void check_heap(void)
 { /* ensure heap order is maintained */
  int lv;
  return;

  for (lv = 1; lv &lt; hsize; lv++)
   {
    if (cost[heap[lv]] &lt; cost[heap[(lv-1)/2]])
     {
      fprintf (stderr, &quot;HEAP ERROR!\n&quot;);
      return;
     }
   }
 }

/* delete the minimum element in the heap */
void delete_min(void)
 {
  int loc, val;
  int p, t;

  /* take last item from the heap */
  loc = heap[--hsize];
  val = cost[loc];

  /* p is the current position of item (loc,val) in the heap */
  /* the item isn't actually there, but that's where we're
     considering putting it */
  p = 0; 

  while (2*p+1 &lt; hsize)
   { /* while one child is less than the last item,
        move the lesser child up */
    t = 2*p+1;
    /* pick lesser child */
    if (t+1 &lt; hsize &amp;&amp; cost[heap[t+1]] &lt; cost[heap[t]]) t++;

    if (cost[heap[t]] &lt; val)
     { /* if child is less than last item, move it up */
      heap[p] = heap[t];
      hloc[heap[p]] = p;
      p = t;
     } else break;
   }

  /* put the last item back into the heap */
  heap[p] = loc;
  hloc[loc] = p;
  check_heap();
 }

/* we decreased the value corresponding to basket loc */
/* alter heap to maintain heap order */
void update(int loc)
 {
  int val;
  int p, t;

  val = cost[loc];
  p = hloc[loc];

  while (p &gt; 0) /* while it's not at the root */
   {
    t = (p-1)/2; /* t = parent of node */
    if (cost[heap[t]] &gt; val)
     { /* parent is higher cost than us, swap */
      heap[p] = heap[t];
      hloc[heap[p]] = p;
      p = t;
     } else break;
   }

  /* put basket back into heap */
  heap[p] = loc;
  hloc[loc] = p;
  check_heap();
 }

/* add this element into the heap */
void add_heap(int loc)
 {
  if (hloc[loc] == -1)
   { /* if it's not in the heap */

    /* add it to the end (same as provisionally setting it's value
       to infinity) */
    heap[hsize++] = loc;
    hloc[loc] = hsize-1;
   }

  /* set to correct value */
  update(loc);
 }

/* given an id, calculate the index of it */
int find_item(int id)
 {
  if (itemid[0] == id) return 0;
  if (itemid[1] == id) return 1;
  if (itemid[2] == id) return 2;
  if (itemid[3] == id) return 3;
  if (itemid[4] == id) return 4;
  return -1;
 }

/* encoding constants 6^0, 6^1, 6^2, ..., 6^5 */
const int mask[5] = {1, 6, 36, 216, 1296};

void find_cost(void)
 {
  int p;
  int cst;
  int lv, lv2;
  int amt;
  offer_t *o;
  int i;
  int t;

  /* initialize costs to be infinity */
  for (lv = 0; lv &lt; 7776; lv++) cost[lv] = 999*25+1;

  /* offer not in heap yet */
  for (lv = 0; lv &lt; 7776; lv++) hloc[lv] = -1;

  /* add empty baset */
  cost[0] = 0;
  add_heap(0);

  while (hsize)
   {
    /* take minimum basket not checked yet */
    p = heap[0];
    cst = cost[p];

    /* delete it from the heap */
    delete_min();

    /* try adding each offer to it */
    for (lv = 0; lv &lt; noffer; lv++)
     {
      o = &amp;offers[lv];
      t = p; /* the index of the new heap */
      for (lv2 = 0; lv2 &lt; o-&gt;nitem; lv2++)
       {
        i = o-&gt;itemid[lv2];
	/* amt = amt of item lv2 already in basket */
	amt = (t / mask[i]) % 6;

	if (amt + o-&gt;itemamt[lv2] &lt;= 5)
	  t += mask[i] * o-&gt;itemamt[lv2];
	else
	 { /* if we get more than 5 items in the basket,
	      this is an illegal move */
	  t = 0; /* ensures we will ignore it, since cost[0] = 0 */
	  break;
	 }
       }
      if (cost[t] &gt; cst + o-&gt;cost)
       { /* we found a better way to get this basket */

        /* update the cost */
        cost[t] = cst + o-&gt;cost;
	add_heap(t); /* add, if necessary, and reheap */
       }
     }
   }
 }

int main(int argc, char **argv)
 {
  FILE *fout, *fin;
  int lv, lv2; /* loop variable */
  int amt[5]; /* goal amounts of each type */
  int a; /* temporary variable */

  if ((fin = fopen(&quot;shopping.in&quot;, &quot;r&quot;)) == NULL)
   {
    perror (&quot;fopen fin&quot;);
    exit(1);
   }
  if ((fout = fopen(&quot;shopping.out&quot;, &quot;w&quot;)) == NULL)
   {
    perror (&quot;fopen fout&quot;);
    exit(1);
   }

  fscanf (fin, &quot;%d&quot;, &amp;noffer);

  /* read offers */
  for (lv = 0; lv &lt; noffer; lv++)
   {
    fscanf (fin, &quot;%d&quot;, &amp;offers[lv].nitem);
    for (lv2 = 0; lv2 &lt; offers[lv].nitem; lv2++)
      fscanf (fin, &quot;%d %d&quot;, &amp;offers[lv].itemid[lv2], &amp;offers[lv].itemamt[lv2]);
    fscanf (fin, &quot;%d&quot;, &amp;offers[lv].cost);
   }

  /* read item's information */
  fscanf (fin, &quot;%d&quot;, &amp;nitem);
  for (lv = 0; lv &lt; nitem; lv++)
    fscanf (fin, &quot;%d %d %d&quot;, &amp;itemid[lv], &amp;amt[lv], &amp;cost[lv]);

  /* fill in rest of items will illegal data, if necessary */
  for (lv = nitem; lv &lt; 5; lv++) 
   {
    itemid[lv] = -1;
    amt[lv] = 0;
    cost[lv] = 0;
   }

  /* go through offers */
  /* make sure itemid's are of item's in goal basket */
  /* translate itemid's into indexes */
  for (lv = 0; lv &lt; noffer; lv++)
   {
    for (lv2 = 0; lv2 &lt; offers[lv].nitem; lv2++)
     {
      a = find_item(offers[lv].itemid[lv2]);
      if (a == -1)
       { /* offer contains an item which isn't in goal basket */

	/* delete offer */

	/* copy last offer over this one */
        memcpy (&amp;offers[lv], &amp;offers[noffer-1], sizeof(offer_t));
	noffer--;

	/* make sure we check this one again */
	lv--;
	break;
       }
      else
        offers[lv].itemid[lv2] = a; /* translate id to index */
     }
   }

  /* add in the degenerate offers of buying single items 8/
  for (lv = 0; lv &lt; nitem; lv++)
   {
    offers[noffer].nitem = 1;
    offers[noffer].cost = cost[lv];
    offers[noffer].itemamt[0] = 1;
    offers[noffer].itemid[0] = lv;
    noffer++;
   }

  /* find the cost for all baskets */
  find_cost();

  /* calculate index of goal basket */
  a = 0;
  for (lv = 0; lv &lt; 5; lv++)
    a += amt[lv] * mask[lv];

  /* output answer */
  fprintf (fout, &quot;%i\n&quot;, cost[a]);
  return 0;
 }
[/c]

标程2:

[c]
#include &lt;fstream&gt;
#include &lt;cstring&gt;
using namespace std;

ifstream fin (&quot;shopping.in&quot;);
ofstream fout(&quot;shopping.out&quot;);

struct special_offer {
    int n;
    int price;              // the price of that special offer
    struct product {        // for each product we have to keep :
        int id;             // the id of the product
        int items;          // how many items it includes
    } prod[6];
} so[100];                  // here the special offers are kept

int code[1000],             /* Each code is 'hashed' from its real value
                               to a smaller index.  Example :
			       If in the input we have code 111, 934, 55,
			       1, 66 we code them as 1,2,3,4,5\. That is
			       kept in code[1000];
                             */

price[6],                   // the price of each product
many[6];                    // how many of each product are to be bought

int s,                      // the number of special offers
    b;                      // the number of different kinds of products to be bought

int sol[6][6][6][6][6];     // here we keep the price of each configuration

void init() {               // reads the input
    fin&gt;&gt;s;
    for (int i=1;i&lt;=s;i++) {
        fin&gt;&gt;so[i].n;
        for (int j=1;j&lt;=so[i].n;j++)
            fin&gt;&gt;so[i].prod[j].id&gt;&gt;so[i].prod[j].items;
        fin&gt;&gt;so[i].price;
    }
    fin&gt;&gt;b;
    for (int i=1;i&lt;=b;i++) {
        int tmp;
        fin&gt;&gt;tmp;
        code[tmp]=i; // here we convert the code to an id from 1..5
        fin&gt;&gt;many[i];
        fin&gt;&gt;price[i];
    }
}

void solve() { // the procedure that solves the problem
    for (int a=0;a&lt;=many[1];a++)
        for (int b=0;b&lt;=many[2];b++)
            for (int c=0;c&lt;=many[3];c++)
                for (int d=0;d&lt;=many[4];d++)
                    for (int e=0;e&lt;=many[5];e++)
                        if ((a!=0)||(b!=0)||(c!=0)||(d!=0)||(e!=0)) {

      int min=a*price[1]+b*price[2]+c*price[3]+d*price[4]+e*price[5];
	  /* in min we keep the lowest price at which we can buy a items
	     from the 1st type, +b from the 2nd+c of the 3rd... e from the
   	      5th */

      for (int k=1;k&lt;=s;k++) { // for each special offer
          int can=1,hm[6];
          memset(&amp;hm,0,sizeof(hm));
          for (int l=1;l&lt;=so[k].n;l++)
              hm[code[so[k].prod[l].id]]=so[k].prod[l].items;
             if ((hm[1]&gt;a)||(hm[2]&gt;b)||(hm[3]&gt;c)||(hm[4]&gt;d)||(hm[5]&gt;e))
                 can=0;// we check if it is possible to use that offer

             if (can) {        // if possible-&gt; check if it is better
                               // than the current min
                 int pr=so[k].price+sol[a-hm[1]][b-hm[2]][c-hm[3]]
                          [d-hm[4]][e-hm[5]];
                         /* Those which are not included in the special offer */
                 if (pr&lt;min) min=pr;
             }
      }
      sol[a][b][c][d][e]=min;

                        }
}

int main() {
    memset(&amp;so,0,sizeof(so));
    init();
    solve();
    fout&lt;&lt;sol[many[1]][many[2]][many[3]][many[4]][many[5]]&lt;&lt;endl;
    return 0;
}
[/c]
title: USACO - PROB The Longest Prefix
tags:
  - USACO
id: 1767
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-20 18:42:08
---

终于自己想出来了DP

代码：<!--more-->

[c]
/*
ID: lmy0525
PROG: prefix
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

vector &lt;string&gt; prims;
string S;
int mark[200000];

int main() {
    fout.open(&quot;prefix.out&quot;);
    fin.open(&quot;prefix.in&quot;);
    string temp;
    fin &gt;&gt; temp;
    while (temp != &quot;.&quot;)
    {
        prims.push_back(temp);
        fin &gt;&gt; temp;
    }
    while (fin &gt;&gt; temp)
    {
        S += temp;
    }
    for (int i = 0; i &lt; prims.size(); i++)
    {
        int len = prims[i].size();
        if (S.substr(0, len) == prims[i]) mark[len-1] = 1;
    }
    for (int i = 0; i &lt; S.size(); i++)
    {
        if (mark[i] == 1)
        {
            for (int j = 0; j &lt; prims.size(); j++)
            {
                int len = prims[j].size();
                if (S.substr(i+1, len) == prims[j]) mark[i+len] = 1;
            }
        }
    }
    int res = 0;
    for (int i = S.size()-1; i &gt;= 0; i--)
    {
        if (mark[i] == 1)
        {
            res = i+1;
            break;
        }
    }
    fout &lt;&lt; res &lt;&lt; endl;
    return 0; 
}
[/c]

标程1：

[c]
#include &lt;stdio.h&gt;

/* maximum number of primitives */
#define MAXP 200
/* maximum length of a primitive */
#define MAXL 10

char prim[MAXP+1][MAXL+1]; /* primitives */
int nump;                  /* number of primitives */

int start[200001];         /* is this prefix of the sequence expressible? */
char data[200000];         /* the sequence */
int ndata;                 /* length of the sequence */

int main(int argc, char **argv)
 {
  FILE *fout, *fin;
  int best;
  int lv, lv2, lv3;

  if ((fin = fopen(&quot;prim.in&quot;, &quot;r&quot;)) == NULL)
   {
    perror (&quot;fopen fin&quot;);
      exit(1);
   }
  if ((fout = fopen(&quot;prim.out&quot;, &quot;w&quot;)) == NULL)
   {
    perror (&quot;fopen fout&quot;);
    exit(1);
   }

  /* read in primitives */
  while (1)
   {
    fscanf (fin, &quot;%s&quot;, prim[nump]);
    if (prim[nump][0] != '.') nump++;
    else break;
   }

  /* read in string, one line at a time */
  ndata = 0;
  while (fscanf (fin, &quot;%s&quot;, data+ndata) == 1)
    ndata += strlen(data+ndata);

  start[0] = 1;
  best = 0;
  for (lv = 0; lv &lt; ndata; lv++)
    if (start[lv]) 
     { /* for each expressible prefix */
      best = lv; /* we found a longer expressible prefix! */
      /* for each primitive, determine the the sequence starting at
         this location matches it */
      for (lv2 = 0; lv2 &lt; nump; lv2++)
       {
        for (lv3 = 0; lv + lv3 &lt; ndata &amp;&amp;  prim[lv2][lv3] &amp;&amp;
	    prim[lv2][lv3] == data[lv+lv3]; lv3++)
          ;
	if (!prim[lv2][lv3])   /* it matched! */
	  start[lv + lv3] = 1; /* so the expanded prefix is also expressive */
       }
     } 

  /* see if the entire sequence is expressible */
  if (start[ndata]) best = ndata; 

  fprintf (fout, &quot;%i\n&quot;, best);
  return 0;
 }
[/c]

标程2：

[pascal]
(* maximum length of primitive *)
const MAXL=20;

type pnode=^node;
     node=record
            next:array['A'..'Z'] of pnode;
            isthere:boolean;
          end;

var trie:pnode;     (* dictionary *)
    p:pointer;
    fin,fout:text;

(* read space separated word from file *)
function readword:string;
var s:string;
    ch:char;
begin
  read(fin,ch);
  while (not (ch in ['A'..'Z','.'])) do
    read(fin,ch);
  s:='';
  while (ch in ['A'..'Z','.']) do
  begin
    s:=s+ch;
    read(fin,ch);
  end;
  readword:=s;
end;

(* read dictionary *)
procedure readdict;
var n,i,j,l:integer;
    nod:pnode;
    ch,ch2:char;
    s:string;
begin
  new(trie);
  for ch2:='A' to 'Z' do
    trie^.next[ch2]:=nil;
  trie^.isthere:=false;
  s:=readword;
  while s&lt;&gt;'.' do
  begin
    nod:=trie;
    l:=length(s);
    (* save word in reverse order *)
    for j:=l downto 1 do
    begin
      ch:=s[j];
      if nod^.next[ch]=nil then
      begin
        new(nod^.next[ch]);
        for ch2:='A' to 'Z' do
          nod^.next[ch]^.next[ch2]:=nil;
        nod^.next[ch]^.isthere:=false;
		end;
      nod:=nod^.next[ch];
    end;
    nod^.isthere:=true;
    s:=readword;
  end;
end;

procedure compute;
var start:array[0..MAXL] of boolean; (* is this prefix (mod MAXL) expressible ? *)
    data:array[0..MAXL] of char;     (* the sequence (mod MAXL) *)
    i,k:integer;
    ch:char;
    nod:pnode;
    max,cnt:longint;
begin
  start[0]:=true;
  read(fin,ch);
  data[0]:=#0; (* sentinel *)
  i:=1;
  max:=0; cnt:=1;
  while not eof(fin) do
  begin
    if not (ch in ['A'..'Z']) then
	 begin
      read(fin,ch);
      continue;
	 end;
    if i=MAXL then i:=0;  (* i:=i mod MAXL *)
    k:=i;
    data[i]:=ch;
    start[i]:=false;
    nod:=trie;
    (* is there primitive at the and of current prefix ? *)
    while nod&lt;&gt;nil do
    begin
      nod:=nod^.next[data[k]];
      dec(k); if k      if start[k] and (nod&lt;&gt;nil) and nod^.isthere then
      begin
        (* we've found a primitive at the end of prefix and the rest is
           expressible *)
		  start[i]:=true;
        max:=cnt; break;
      end;
      if data[k]=#0 then break;
    end;
    read(fin,ch);

    (* if last MAXL prefixes are not expressible, no other prefix can
	    be expressible *)
    if cnt&gt;max+MAXL then break;

    inc(i); inc(cnt);
  end;

  (* write answer *)
  assign(fout,'prefix.out');
  rewrite(fout);
  writeln(fout,max);
  close(fout);
end;

begin
  mark(p);
  assign(fin,'prefix.in');
  reset(fin);
  readdict;
  compute;
  close(fin);
  release(p);
end.
[/pascal]
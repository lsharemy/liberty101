title: USACO - PROB Shaping Regions
tags:
  - USACO
id: 1798
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-22 20:25:32
---

参考了标程3，神递归。学习了矩形分割的思想。

代码：<!--more-->
[c]
/*
ID: lmy0525
PROG: rect1
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
using namespace std;

ofstream fout;
ifstream fin;

int rect[1001][5];
int rect_up[1001][4];
int color[2501];
int cnt_up;

void calc(int idx, int x1, int y1, int x2, int y2, int c)
{
    if (idx == cnt_up)
    {
        color[c] += (x2-x1+1)*(y2-y1+1);
        return;
    }
    int xx1 = rect_up[idx][0];
    int xx2 = rect_up[idx][2];
    int yy1 = rect_up[idx][1];
    int yy2 = rect_up[idx][3];
    if (x1 &gt; xx2 || x2 &lt; xx1 || y1 &gt; yy2 || y2 &lt; yy1)
    {
        calc(idx+1, x1, y1, x2, y2, c); // no overlap
        return;
    }
    if (y1 &lt; yy1) calc(idx+1, x1, y1, x2, yy1-1, c); // bottom
    if (y2 &gt; yy2) calc(idx+1, x1, yy2+1, x2, y2, c); // top
    if (x1 &lt; xx1) calc(idx+1, x1, y1&gt;yy1?y1:yy1, xx1-1, y2&lt;yy2?y2:yy2, c); // left
    if (x2 &gt; xx2) calc(idx+1, xx2+1, y1&gt;yy1?y1:yy1, x2, y2&lt;yy2?y2:yy2, c); // right
}

int main()
{
    fout.open(&quot;rect1.out&quot;);
    fin.open(&quot;rect1.in&quot;);
    int A, B, N;
    fin &gt;&gt; A &gt;&gt; B &gt;&gt; N;
    rect[0][0] = rect[0][1] = 0;
    rect[0][2] = A;
    rect[0][3] = B;
    rect[0][4] = 1;
    for (int i = 1; i &lt;= N; i++)
    {
        fin &gt;&gt; rect[i][0] &gt;&gt; rect[i][1] &gt;&gt; rect[i][2] &gt;&gt; rect[i][3] &gt;&gt; rect[i][4];
    }

    for (int i = N; i &gt;= 0; i--)
    {
        calc(0, rect[i][0], rect[i][1], rect[i][2]-1, rect[i][3]-1, rect[i][4]);
        rect_up[cnt_up][0] = rect[i][0];
        rect_up[cnt_up][1] = rect[i][1];
        rect_up[cnt_up][2] = rect[i][2]-1;
        rect_up[cnt_up][3] = rect[i][3]-1;
        cnt_up++;
    }

    for (int i = 0; i &lt;= 2500; i++)
    {
        if (color[i])
        {
            fout &lt;&lt; i &lt;&lt; &quot; &quot; &lt;&lt;color[i] &lt;&lt; endl;
        }    
    }
    return 0;
}
[/c]

标程1：
[c]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;

FILE *fp,*fo;

struct rect
{
    int c;
    int x1,y1,x2,y2;
};

int c[2501];
rect r[10001];

int intersect(rect a,const rect &amp;b,rect out[4])
{
    /* do they at all intersect? */
    if(b.x2&lt;a.x1||b.x1&gt;=a.x2)
        return 0;
    if(b.y2&lt;a.y1||b.y1&gt;=a.y2)
        return 0;
    /* they do */

    rect t;

    if(b.x1&lt;=a.x1&amp;&amp;b.x2&gt;=a.x2&amp;&amp;b.y1&lt;=a.y1&amp;&amp;b.y2&gt;=a.y2)
            return -1;

    /* cutting `a' down to match b */
    int nout=0;
    if(b.x1&gt;=a.x1) {
        t=a,t.x2=b.x1;
        if(t.x1!=t.x2)
            out[nout++]=t;
        a.x1=b.x1;
    }
    if(b.x2&lt;a.x2) {
        t=a,t.x1=b.x2;
        if(t.x1!=t.x2)
            out[nout++]=t;
        a.x2=b.x2;
    }
    if(b.y1&gt;=a.y1) {
        t=a,t.y2=b.y1;
        if(t.y1!=t.y2)
            out[nout++]=t;
        a.y1=b.y1;
    }
    if(b.y2&lt;a.y2) {
        t=a,t.y1=b.y2;
        if(t.y1!=t.y2)
            out[nout++]=t;
        a.y2=b.y2;
    }
    return nout;
}

int main(void) {
    fp=fopen(&quot;rect1.in&quot;,&quot;rt&quot;);
    fo=fopen(&quot;rect1.out&quot;,&quot;wt&quot;);

    int a,b,n;
    fscanf(fp,&quot;%d %d %d&quot;,&amp;a,&amp;b,&amp;n);

    r[0].c=1;
    r[0].x1=r[0].y1=0;
    r[0].x2=a;
    r[0].y2=b;

    rect t[4];

    int i,j,rr=1;
    for(i=0;i&lt;n;i++) {
        int tmp;
        fscanf(fp,&quot;%d %d %d %d %d&quot;,&amp;r[rr].x1,&amp;r[rr].y1,&amp;r[rr].x2,&amp;r[rr].y2,&amp;r[rr].c);

        if(r[rr].x1&gt;r[rr].x2) {
            tmp=r[rr].x1;
            r[rr].x1=r[rr].x2;
            r[rr].x2=tmp;
        }
        if(r[rr].y1&gt;r[rr].y2) {
            tmp=r[rr].y1;
            r[rr].y1=r[rr].y2;
            r[rr].y2=tmp;
        }

        int nr=rr;
        rect curr=r[rr++];
        for(j=0;j&lt;nr;j++) {
            int n=intersect(r[j],curr,t);
            if(!n)
                continue;
            if(n==-1) {
                memmove(r+j,r+j+1,sizeof(rect)*(rr-j-1));
                j--;
                rr--;
                nr--;
                continue;
            }
            r[j]=t[--n];
            for(;n--&gt;0;)
                r[rr++]=t[n];
        }
    }

    for(i=0;i&lt;rr;i++)
        c[r[i].c]+=(r[i].x2-r[i].x1)*(r[i].y2-r[i].y1);

    for(i=1;i&lt;=2500;i++)
        if(c[i])
            fprintf(fo,&quot;%d %d\n&quot;,i,c[i]);

    return 0;
}
[/c]

标程2：
[pascal]
Program rrect1;
  Var
    Inf,Outf            : Text;
    A,B,N,I,Z,Middle,J  : Longint;
    Color               : Array [1..2500] of Longint;
    D                   : Array [1..5000,1..5000] of boolean;
    Xar,Yar             : Array [0..2500] of Longint;
    Col                 : Array [1..10000] of Record
                                               x1 : Longint;
                                               x2 : Longint;
                                               y1 : Longint;
                                               y2 : Longint;
                                               c  : Longint;
                                             End;

  Function Find (K1 : integer) : Longint;
  Var
    Pointer,N1,N2                          : Longint;
  Begin
    N1 := 1;
    N2 := N + N;
    While N1 &gt; 0 Do Begin
        Pointer := (N1 + N2) Div 2;
        If Xar[Pointer] = K1 then Begin
           Find := Pointer;
           Exit;
        End;
        If Xar[Pointer] &gt; K1 Then
          N2 := Pointer - 1;
        If Xar[Pointer] &lt; K1 Then
          N1 := Pointer + 1;
    End;
  End;

  Function Find1 (K2 : Longint) : Longint;
  Var
    Pointer,N1,N2                          : Longint;
  Begin
    N1 := 1;
    N2 := N + N;
    While N1 &gt; 0 Do Begin
        Pointer := (N1 + N2) Div 2;
        If Yar[Pointer] = K2 then Begin
           Find1 := Pointer;
           Exit;
        End;
        If Yar[Pointer] &gt; K2 Then
          N2 := Pointer - 1;
        If Yar[Pointer] &lt; K2 Then
          N1 := Pointer + 1;
    End;
  End;

  Procedure Partition1 ( Lf , Rg : Longint );
  Var
    Pivot,L,R,Temp                 : Longint;
  Begin
    Pivot := Yar[Lf];
    L := Lf;
    R := Rg;
    While L &lt; R Do Begin
        While (Yar[L] &lt;= Pivot) and (L &lt;= R) Do Inc(L);
        While (Yar[R] &gt; Pivot) And (R &gt;= L) Do Dec(R);
        If L &lt; R Then
          begin
            Temp := Yar[L];
            Yar[L] := Yar[R];
            Yar[R] := Temp;
          end;
      End;
      Middle := R;
      Temp := Yar[Lf];
      Yar[Lf] := Yar[R];
      Yar[R] := Temp;
  End;

  Procedure QSort1 ( Left , Right : Longint );
  Begin
    if Left &lt; Right Then
      Begin
        Partition1 (Left,Right);
        QSort1 (Left,Middle-1);
        QSort1 (Middle + 1, Right);
      End;
  End;

  Procedure Partition ( Lf , Rg : Longint );
  Var
    Pivot,L,R,Temp              : Longint;
  Begin
    Pivot := Xar[Lf];
    L := Lf;
    R := Rg;
    While L &lt; R Do
      Begin
        While (Xar[L] &lt;= Pivot) and (L &lt;= R) Do Inc(L);
        While (Xar[R] &gt; Pivot) And (R &gt;= L) Do Dec(R);
        If L &lt; R Then
          begin
            Temp := Xar[L];
            Xar[L] := Xar[R];
            Xar[R] := Temp;
          end;
      End;
      Middle := R;
      Temp := Xar[Lf];
      Xar[Lf] := Xar[R];
      Xar[R] := Temp;
  End;

  Procedure QSort ( Left , Right : Longint );
  Begin
    if Left &lt; Right Then
      Begin
        Partition (Left,Right);
        QSort (Left,Middle-1);
        QSort (Middle + 1, Right);
      End;
  End;

  Begin
    Assign (Inf ,'rect1.in');
    Reset (Inf);
    Readln (Inf,A,B,N);
    For I := 1 To N Do
        Readln (Inf , Col[I].x1,col[i].y1,col[i].x2,col[i].y2,col[i].c);
    Close (Inf);
    For I := 1 to 2500 do Color[I] := 0;
    Color[1] := A * B;
    For I := 0 to n do
      For J := 0 to n do
        D[I,J] := False;
    Xar[0] := 0;
    Xar[n+n+1] := a;
    For I := 1 To N do Xar[i] := Col[i].x1;
    For I := N + 1 To N + N do Xar[i] := Col[i - n].x2;
      Qsort (1,N + N);
    Yar[0] := 0;
    Yar[n+n+1] := b;
    For I := 1 To N Do Yar[i] := Col[i].y1;
    For I := N + 1 To N + N do Yar[i] := col[i - n].y2;
      Qsort1 (1,N + N);
    For I := N Downto 1 Do
      For J := find(Col[i].x1) + 1 to find(col[i].x2) do
        For Z := find1(col[i].y1) + 1 to find1(col[i].y2) do
          If not D[J,Z] then
            Begin
              If col[i].c &gt; 1 then
                Begin
                  Middle := (Xar[j] - Xar[j-1]) * (Yar[z] - Yar[z-1]);
                  Color[Col[i].c] := color[Col[i].c] + Middle;
                  Color[1] := Color[1] - Middle;
                End;
              D[J,Z] := True;
            End;
    Assign (Outf, 'rect1.out');
    Rewrite (Outf);
    For I := 1 To 2500 Do
      if color[i] &gt; 0 then
        Writeln (Outf,i,' ', Color[i]);
    Close(Outf);
  End.
[/pascal]

标程3：
[pascal]
program rect1;

var F: Text;
    i,a,b,n,cused,maxcolor:word;
    inform: array[1..1001,1..5] of word;
    used: array[1..1001,1..4] of word;
    countcolor: array[1..2500] of longint;

procedure cac(count,x1,y1,x2,y2,color: word); //cut and count
var py1,py2 : word;
begin
    if count&lt;cused then begin
        if (x1&gt;used[count,3]) or (x2&lt;used[count,1]) or (y1&gt;used[count,4]) or (y2&lt;used[count,2]) then
            cac(succ(count),x1,y1,x2,y2,color)  //if there are no difficulties with the other rectangle
        else begin
            if y1&gt;used[count,2] then py1:=y1 else py1:=used[count,2];
            if y2&lt;used[count,4] then py2:=y2 else py2:=used[count,4];
            if y1&lt;used[count,2] then cac(succ(count),x1,y1,x2,pred(used[count,2]),color);
            if y2&gt;used[count,4] then cac(succ(count),x1,succ(used[count,4]),x2,y2,color);
            if x1&lt;used[count,1] then cac(succ(count),x1,py1,pred(used[count,1]),py2,color);
            if x2&gt;used[count,3] then cac(succ(count),succ(used[count,3]),py1,x2,py2,color);
        end;
    end else inc(countcolor[color],succ(x2-x1)*succ(y2-y1));
end;

begin
    Assign(F,'rect1.in');
    Reset(F);
    Readln(F,a,b,n);
    inc(n);
    for i:=2 to n do
        Readln(F,inform[i,1],inform[i,2],inform[i,3],inform[i,4],inform[i,5]);  //x1,y1,x2,y2,color
    Close(F);
    inform[1,1]:=0; inform[1,2]:=0; //white paper
    inform[1,3]:=a; inform[1,4]:=b; inform[1,5]:=1;
    maxcolor:=1;
    cused:=1;

    for i:=n downto 1 do begin
        cac(1,inform[i,1],inform[i,2],pred(inform[i,3]),pred(inform[i,4]),inform[i,5]);
        if inform[i,5]&gt;maxcolor then maxcolor:=inform[i,5];  //we don't have to check all 2500 colors
        used[cused,1]:=inform[i,1];  //saving the coordinates of the rectangle
        used[cused,2]:=inform[i,2];
        used[cused,3]:=pred(inform[i,3]);
        used[cused,4]:=pred(inform[i,4]);
        inc(cused);
    end;

    Assign(F,'rect1.out');
    Rewrite(F);
    for i:=1 to maxcolor do
        if countcolor[i]&gt;0 then
            Writeln(F,i,' ',countcolor[i]);
  Close(F);
end.
[/pascal]
title: itoa和atoi
tags:
  - C
id: 1971
categories:
  - wordpress
  - index.php
  - csit
date: 2012-11-04 16:48:49
---

再贴个自己写的itoa和atoi：

[c]
#include &lt;cstdio&gt;
#include &lt;cstring&gt;
#include &lt;cctype&gt;

void reverse(char s[])
{
    int n = strlen(s);
    for (int i = 0, j = n-1; i &lt; j; i++, j--)
    {
        char c = s[i];
        s[i] = s[j];
        s[j] = c;
    }
}

int atoi(char s[])
{
    int i, n, sign;
    for (i = 0; isspace(s[i]); i++)
        ;
    sign = (s[i] == '-')?-1:1;
    if (s[i] == '-' || s[i] == '+')
        i++;
    for (n = 0;  isdigit(s[i]); i++)
        n = 10*n + (s[i] - '0');
    return n*sign;
}

void itoa(int n, char s[])
{
    int i = 0, sign = n;
    if (n &lt; 0) n = -n;
    do
    {
        s[i++] = n%10 + '0';
        n /= 10;
    }while(n);
    if (sign &lt; 0) s[i++] = '-';
    s[i] = '&#92;&#48;';
    reverse(s);
}

int main()
{
    char s[20];
    printf(&quot;%d\n&quot;, atoi(&quot;-525&quot;));
    itoa(-525, s);
    printf(&quot;%s\n&quot;, s);
}
[/c]

atoi是stdlib.h中的函数，而itoa不属于标准函数，我的atoi没有考虑溢出问题，如果要考虑，写起来还挺麻烦的，具体可以参考标准库中的实现。

itoa上次面试的时候碰到了，没有想到逆序输出，最后reverse这种方法，这个方法是在K&R的经典C中发现的，看来以前看书不认真啊= =
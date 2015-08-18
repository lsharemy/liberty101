title: 逗号表达式陷阱
tags:
  - C
id: 1956
categories:
  - wordpress
  - index.php
  - csit
date: 2012-10-22 09:50:29
---

以下程序会输出什么：

[c]
#include &lt;cstdio&gt;

using namespace std;

int main(void)
{
    int a = 1, b = 1, c;
    c = a++, b++, ++b;
    int d = (a, b++);
    printf(&quot;%d %d %d %d\n&quot;, a, b, c, d);
}
[/c]

答案是（选中下行查看）：
<pre><span style="color: #000000;">2 4 1 3</span></pre>
逗号表达式的要领：
(1) 逗号表达式的运算过程为：从左往右逐个计算表达式。
(2) 逗号表达式作为一个整体，它的值为最后一个表达式（也即表达式n）的值。
(3) 逗号运算符的优先级别在所有运算符中最低。
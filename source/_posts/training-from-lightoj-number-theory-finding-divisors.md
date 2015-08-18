title: 'Training from LightOJ | Number Theory - Finding Divisors'
tags:
  - LightOJ
id: 785
categories:
  - wordpress
  - index.php
  - csit
date: 2011-12-21 23:34:12
---

# <span style="color: #ff0000;">寻找因子</span>

跟寻找质因子一样，也是试除法。多于整数n，也只需要试除到sqrt(n)为止，因为因子总是成对的。比如36的因子有1、2、3、4、6、9、12、18、36。只要找到2，我们也就找到了36/2=18，找到3，也就找到了36/3=12，所以<!--more-->只要检测到sqrt(n)为止就好了。代码如下：

[c]
#include &lt;cstdio&gt;
#include &lt;set&gt;
#include &lt;cmath&gt;

using namespace std;

int main()
{
    set &lt;int&gt; divisor;
    int n, i;
    scanf(&quot;%d&quot;, &amp;n);
    int sqrtN = sqrt(n);
    for (i = 1; i &lt;= sqrtN; i++)
    {
        if (n % i == 0)
        {
            divisor.insert(i);
            divisor.insert(n/i);
        }
    }

    set&lt;int&gt;::iterator it;
    for (it = divisor.begin(); it != divisor.end(); it++)
        printf(&quot;%d &quot;, *it);
    printf(&quot;\ndivisor count: %d&quot;, divisor.size());
}
[/c]

既然我们已经找到了所有因子，因子的个数通过计数就可以得到了，所有因子的和也可以通过把它们全部加起来得到。

# <span style="color: #ff0000;">寻找因子个数</span>

上面通过找出所有因子，最后得到因子个数。不过还有更简单的方法来得到因子的个数。这个方法需要先对数n进行质因数分解。比如，对于18，质因数分解为：18 = 2<sup>1</sup> * 3<sup>2</sup>

那么18的所有因子可以表示为：2<sup>x</sup> * 3<sup>y</sup> 其中0 &lt;= x &lt;= 1 and 0 &lt;= y &lt;= 2

即如下6个：
2<sup>0</sup> * 3<sup>0</sup> = 1
2<sup>0</sup> * 3<sup>1</sup> = 3
2<sup>0</sup> * 3<sup>2</sup> = 9
2<sup>1</sup> * 3<sup>0</sup> = 2
2<sup>1</sup> * 3<sup>1</sup> = 6
2<sup>1</sup> * 3<sup>2</sup> = 18

可以推出，如果一个数可以质因数分解成2<sup>x</sup> * 3<sup>y</sup>，那么它的因子个数为(x+1)*(y+1)。

更一般的，如果一个数可以质因数分解为p<sub>1</sub><sup>x1</sup> * p<sub>2</sub><sup>x2</sup> * p<sub>3</sub><sup>x3</sup> * ... * p<sub>n</sub><sup>xn</sup>

那么它的因子个数为(x<sub>1</sub> + 1) * (x<sub>2</sub> + 1) * (x<sub>3</sub> + 1) * ... * (x<sub>n</sub> + 1)

代码如下：

[c]
#include &lt;cstdio&gt;
#include &lt;set&gt;
#include &lt;cmath&gt;

using namespace std;

int prime[32];
int factor[32];
int cnt;

int main()
{
    int n;
    scanf(&quot;%d&quot;, &amp;n);
    cnt = 0;
    for (int i = 2; i*i &lt;= n; i++)
    {
        if (n % i == 0)
        {
            factor[cnt] = 0;
            prime[cnt] = i;
            while (n % i == 0)
            {
                n /= i;
                factor[cnt]++;
            }
            cnt++;
        }
    }
    if ( n &gt; 1)
    {
        prime[cnt] = n;
        factor[cnt++] = 1;
    }

    int divisorCnt = 1;
    for (int i = 0; i &lt; cnt; i++)
        divisorCnt *= factor[i]+1;
    printf(&quot;%d&quot;, divisorCnt);
}
[/c]

# <span style="color: #ff0000;">计算所有因子的和</span>

正如前面所说，得到了所有的因子之后，把它们全部加起来就得到了和。不过有更好的办法。也需要用到质因数分解。比如对于60。因子有1, 2, 3, 4, 5, 6, 10, 12, 15, 20, 30, 60。它们的和为168。也可以写成：

2<sup>0</sup> * 3<sup>0</sup> * 5<sup>0</sup> + 2<sup>0</sup> * 3<sup>0</sup> * 5<sup>1</sup> + 2<sup>0</sup> * 3<sup>1</sup> * 5<sup>0</sup> + 2<sup>0</sup> * 3<sup>1</sup> * 5<sup>1</sup> +

2<sup>1</sup> * 3<sup>0</sup> * 5<sup>0</sup> + 2<sup>1</sup> * 3<sup>0</sup> * 5<sup>1</sup> + 2<sup>1</sup> * 3<sup>1</sup> * 5<sup>0</sup> + 2<sup>1</sup> * 3<sup>1</sup> * 5<sup>1</sup> +

2<sup>2</sup> * 3<sup>0</sup> * 5<sup>0</sup> + 2<sup>2</sup> * 3<sup>0</sup> * 5<sup>1</sup> + 2<sup>2</sup> * 3<sup>1</sup> * 5<sup>0</sup> + 2<sup>2</sup> * 3<sup>1</sup> * 5<sup>1</sup>

化简为：

2<sup>0</sup> * (3<sup>0</sup> * 5<sup>0</sup> + 3<sup>0</sup> * 5<sup>1</sup> + 3<sup>1</sup> * 5<sup>0</sup> + 3<sup>1</sup> * 5<sup>1</sup>) +

2<sup>1</sup> * (3<sup>0</sup> * 5<sup>0</sup> + 3<sup>0</sup> * 5<sup>1</sup> + 3<sup>1</sup> * 5<sup>0</sup> + 3<sup>1</sup> * 5<sup>1</sup>) +

2<sup>2</sup> * (3<sup>0</sup> * 5<sup>0</sup> + 3<sup>0</sup> * 5<sup>1</sup> + 3<sup>1</sup> * 5<sup>0</sup> + 3<sup>1</sup> * 5<sup>1</sup>)

继续化简：

(2<sup>0</sup> + 2<sup>1</sup> + 2<sup>2</sup>) * (3<sup>0</sup> * 5<sup>0</sup> + 3<sup>0</sup> * 5<sup>1</sup> + 3<sup>1</sup> * 5<sup>0</sup> + 3<sup>1</sup> * 5<sup>1</sup>)

最后得到：

(2<sup>0</sup> + 2<sup>1</sup> + 2<sup>2</sup>) * (3<sup>0</sup> + 3<sup>1</sup>) * (5<sup>0</sup> + 5<sup>1</sup>)

利用公式：1+r<sup>1</sup> + r<sup>2</sup> + ... + r<sup>n</sup> = (r<sup>n</sup> - 1)/(r - 1)

所以如果一个整数的质因数分解为p<sub>1</sub><sup>x1</sup> * p<sub>2</sub><sup>x2</sup> * p<sub>3</sub><sup>x3</sup> * ... * p<sub>n</sub><sup>xn</sup>

则它的所有因子的和为：[(p<sub>1</sub><sup>x1+1 </sup>- 1)/(p<sub>1 </sub>- 1)] * [(p<sub>2</sub><sup>x2</sup> - 1)/(p<sub>2 </sub>- 1)] * [(p<sub>3</sub><sup>x3</sup> - 1)/p3<sub> </sub>- 1)] * ... * [(p<sub>n</sub><sup>xn</sup> - 1)/pn<sub> </sub>- 1)]

代码如下（只可以在小整的情况下使用，而且用了递归，效率应该也不高，水平有限，以后再回来改进）

[c]
#include &lt;cstdio&gt;
#include &lt;cmath&gt;

using namespace std;

int prime[32];
int factor[32];
int cnt;

//求解 x^n
int Pow(int x,int n)
{
    int ret=1,s=x;
    while(1)
    {
        if(n&amp;1)
            ret=ret*s;
        if(n&gt;&gt;=1)
            s=(s*s);
        else
            break;
    }
    return ret;
}

//求解 1+p+p^2+...+p^n 等比数列的求和
int Sum(int p, int n)
{
    if(n==0)
        return 1;
    if(n&amp;1)
        return ((1+Pow(p,n/2+1))*Sum(p,n/2));
    else
        return ((1+Pow(p,n/2+1))*Sum(p,(n-1)/2)+Pow(p,n/2));
}

int main()
{
    int n;
    scanf(&quot;%d&quot;, &amp;n);
    cnt = 0;
    for (int i = 2; i*i &lt;= n; i++)
    {
        if (n % i == 0)
        {
            factor[cnt] = 0;
            prime[cnt] = i;
            while (n % i == 0)
            {
                n /= i;
                factor[cnt]++;
            }
            cnt++;
        }
    }
    if ( n &gt; 1)
    {
        prime[cnt] = n;
        factor[cnt++] = 1;
    }

    int divisorSum = 1;
    for (int i = 0; i &lt; cnt; i++)
        divisorSum *= Sum (prime[i], factor[i]);
    printf(&quot;%d&quot;, divisorSum);
}
[/c]
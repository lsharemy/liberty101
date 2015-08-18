title: 'Training from LightOJ | Number Theory -  Generating Primes (Part 1) 质数（素数）的生成'
tags:
  - LightOJ
id: 762
categories:
  - wordpress
  - index.php
  - csit
date: 2011-12-20 00:14:55
---

# 质数（素数）的生成

# <span style="color: #ff0000;">**简介**</span>

质数（素数）：只能被两个不同整数整除的数。1不是质数，因为它只能被1个数整数。2和5是指数，因为它们只能被1和本身整除。

要生成1到N之间的素数，我们该怎么<!--more-->做呢？

# <span style="color: #ff0000;">**方法一（试除所有可能的数）**</span>

对于每一个数n，试除从2到n-1，代码如下：

[c]
#include &lt;cstdio&gt;

int N = 5000;

bool isPrime( int n )
{
    for (int i = 2; i &lt; n; i++)
        if (n % i == 0) // n is divisible by i, so n is not a prime
            return false;
    return true; // No integer less than i, divides i, so, i is a prime
}

int main()
{
    for (int i = 2; i &lt;= N; i++)
    {
        if (isPrime(i))
            printf(&quot;%d &quot;, i);
    }
}
[/c]

这个方法很直观，不过也很费时，下面想办法避免一些不必要的检查和试除。当N=10000时，所需要的时间为4秒多（因为printf函数比较费时，时间是注释掉该函数后测出来的，后面的所有时间都是指注释掉printf函数跑出来的时间，而且是用codeblock自带的计时功能）。

# <span style="color: #ff0000;">**方法二（试除到平方根）**</span>

对于每个数n，其实只要试除2到n的平方根就好了，可以消除不必要的试除，所以修改代码如下：

[c]
#include &lt;cstdio&gt;
#include &lt;cmath&gt;

int N = 5000;

bool isPrime( int n )
{
    int sqrtN = sqrt(n);
    // dont write &quot;for(int j = 2; j &lt;= sqrt(i); j++)&quot; because sqrt is a slow
    // function. So, dont calculate it all the time, calculate it only once
    for (int i = 2; i &lt;= sqrtN; i++)
        if (n % i == 0) // n is divisible by i, so n is not a prime
            return false;
    return true; // No integer less than i, divides i, so, i is a prime
}

int main()
{
    for (int i = 2; i &lt;= N; i++)
    {
        if (isPrime(i))
            printf(&quot;%d &quot;, i);
    }
}
[/c]

当N=1000000时，跑出来的时间为0.6秒多，可以看到比起方法一，这个算法已经快了很多很多了（N大了一个数量级，最后的时间还小了两个数量级）。

# <span style="color: #ff0000;">**方法三（没必要考虑偶数）**</span>

由于2是唯一的质数，所以我们可以不检查偶数（condition 1），试除的时候也不在试除偶数（condition 2），因为奇数只能被奇数整除。

[c]
#include &lt;cstdio&gt;
#include &lt;cmath&gt;

int N = 5000;

bool isPrime( int n )
{
    int sqrtN = sqrt(n);
    for (int i = 3; i &lt;= sqrtN; i += 2) // i += 2 is given, condition (2)
        if (n % i == 0) // n is divisible by i, so n is not a prime
            return false;
    return true;
}

int main()
{
    printf(&quot;2 &quot;); // 2 is the only even prime, so, print it
    for (int i = 3; i &lt;= N; i += 2) // i += 2 is given here, condition (1)
    {
        if (isPrime(i))
            printf(&quot;%d &quot;, i);
    }
}
[/c]

当N=1000000时，运行时间为0.3秒多，比起方法二，快了一倍左右。

# <span style="color: #ff0000;">**方法四（埃拉托色尼筛选法）**</span>

原理简单可以描述为，如果一个数不能被任何小于它的质数整除，那么它是一个质数。具体参考：[http://en.wikipedia.org/wiki/Sieve_of_Eratosthenes](http://en.wikipedia.org/wiki/Sieve_of_Eratosthenes)

[c]
#include &lt;cstdio&gt;

int N = 5000;
int status[5001];
// status[i] = 0, if i is prime
// status[i] = 1, if i is not a prime
int main()
{
    // initially we think that all are primes, so change the status
    for (int i = 2; i &lt;= N; i++)
        status[i] = 0;

    for (int i = 2; i &lt;= N; i++) // i += 2 is given here, condition (1)
    {
        if (status[i] == 0)
        {
            // so, i is a prime, so, discard all the multiples
            // j = 2 * i is the first multiple, then j += i, will find the
            // next multiple
            for (int j = 2*i; j &lt;= N; j += i)
                status[j] = 1; // status of the multiple is 1
        }
    }
    // print the primes
    for (int i = 2; i &lt;= N; i++)
        if (status[i]  == 0)
            printf(&quot;%d &quot;, i); // so, i is prime
}
[/c]

N=100000时0.1秒，比起试除法，埃拉托色尼筛选法又快了不少，当N=10000000时，需要1.1秒左右。

# <span style="color: #ff0000;">**方法五（不考虑偶数的埃拉托色尼筛选法）**</span>

代码修改如下：

[c]
#include &lt;cstdio&gt;

int N = 5000;
int status[5001];
// status[i] = 0, if i is prime
// status[i] = 1, if i is not a prime
int main()
{
    // initially we think that all are primes
    for (int i = 2; i &lt;= N; i++)
        status[i] = 0;

    for (int i = 3; i &lt;= N; i += 2) // i += 2 is given here, condition (1)
    {
        if (status[i] == 0)
        {
            // so, i is a prime, so, discard all the multiples
            // 3 * i is odd, since i is odd. And j += 2 * i, so, the next odd
            // number which is multiple of i will be found
            for (int j = 3*i; j &lt;= N; j += 2*i)
                status[j] = 1; // status of the multiple is 1
        }
    }
    // print the primes
    printf(&quot;2 &quot;);
    for (int i = 3; i &lt;= N; i += 2)
        if (status[i]  == 0)
            printf(&quot;%d &quot;, i); // so, i is prime
}
[/c]

当N=10000000时0.7秒左右。

# <span style="color: #ff0000;">**方法六（再度改进埃拉托色尼筛选法）**</span>

只需筛选到N的平方根。如果n是质数，第一个被筛的数应该从n*n开始。

[c]
#include &lt;cstdio&gt;
#include &lt;cmath&gt;

int N = 5000;
int status[5001];
// status[i] = 0, if i is prime
// status[i] = 1, if i is not a prime
int main()
{
    for (int i = 3; i &lt;= N; i++) status[i] = 0;

    int sqrtN = sqrt(N);
    for (int i = 3; i &lt;= sqrtN; i += 2) // have to check primes up to (sqrt(N))
    {
        if (status[i] == 0)
        {
            // so, i is a prime, so, discard all the multiples
            // j = i * i, because it's the first number to be colored
            for (int j = i*i; j &lt;= N; j += i + i)
                status[j] = 1; // status of the multiple is 1
        }
    }
    // print the primes
    printf(&quot;2 &quot;);
    for (int i = 3; i &lt;= N; i += 2)
        if (status[i]  == 0)
            printf(&quot;%d &quot;, i); // so, i is prime
}
[/c]

当N=10000000时0.5秒不到。

# <span style="color: #ff0000;">**方法七（节省内存的方法）**</span>

既然不用考虑偶数，那么也不用为偶数分配内存了，所以可以节省一半的内存。

[c]
#include &lt;cstdio&gt;
#include &lt;cmath&gt;

int N = 5000;
int status[2501]; // save odd numbers only
// status[i] = 0, if 2*i+1 is prime
// status[i] = 1, if 2*i+1 is not a prime
int main()
{
    for (int i = 1; i &lt;= N&gt;&gt;1; i++) status[i] = 0;

    int sqrtN = sqrt(N);
    for (int i = 3; i &lt;= sqrtN; i += 2) // have to check primes up to (sqrt(N))
    {
        if (status[i&gt;&gt;1] == 0)
        {
            // so, i is a prime, so, discard all the multiples
            // j = i * i, because it's the first number to be colored
            for (int j = i*i; j &lt;= N; j += i + i)
                status[j&gt;&gt;1] = 1; // status of the multiple is 1
        }
    }
    // print the primes
    printf(&quot;2 &quot;);
    for (int i = 3; i &lt;= N; i += 2)
        if (status[i&gt;&gt;1]  == 0)
            printf(&quot;%d &quot;, i); // so, i is prime
}
[/c]

当N=10000000时0.4秒左右，比起方法六，节省了一半内存，时间上差不多:-D
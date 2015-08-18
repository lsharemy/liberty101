title: 'Training from LightOJ | Number Theory - Prime Factorization 质因数分解'
tags:
  - LightOJ
id: 777
categories:
  - wordpress
  - index.php
  - csit
date: 2011-12-20 23:34:00
---

# **质因数分解**

# <span style="color: #ff0000;">**简介**</span>

质因数分解就是这样：_4021920 = 2<sup>5</sup> * 3<sup>3</sup> * 5 * 7<sup>2</sup> * 19_

如何进行质因数分解？首先我们要生成质数，这个在[上一篇Training](http://lsharemy.com/wordpress/index.php/csit/training-from-lightoj-number-theory-generating-primes/)里讲过了。

# <span style="color: #ff0000;">**方法一**</span>

最容易想到的办法就是试除所有质数，<!--more-->直到这个数变为1。如40，先除以2，变成20，继续除2，变成10，继续除2，变成5，不能被2除了，检查3，也不能，然后5，除以5后，就变成1了。所以40=2*2*2*5=2<sup>3</sup>*5。代码如下（其中生成质数的代码用的[上篇文章](http://lsharemy.com/wordpress/index.php/csit/training-from-lightoj-number-theory-generating-primes/)的方法7）：

[c]
#include &lt;cstdio&gt;
#include &lt;cmath&gt;

int N = 10000000;
int status[5000001];
int prime[664579];

void primeGeneration()
{
    for (int i = 1; i &lt;= N&gt;&gt;1; i++) status[i] = 0;

    int sqrtN = sqrt(N);
    for (int i = 3; i &lt;= sqrtN; i += 2)
    {
        if (status[i&gt;&gt;1] == 0)
        {
            for (int j = i*i; j &lt;= N; j += i + i)
                status[j&gt;&gt;1] = 1;
        }
    }

    prime[0] = 2;
    int cnt = 1;
    for (int i = 3; i &lt;= N; i += 2)
        if (status[i&gt;&gt;1]  == 0)
            prime[cnt++] = i;
}

int List[32]; // saves the List
int listSize; // saves the size of List

void primeFactorize(int n)
{
    listSize = 0; // Initially the List is empty, we denote that size = 0
    for (int i = 0; prime[i] &lt;= n; i++)
    {
        if (n % prime[i] == 0) // So, n is a multiple of the ith prime
        {
            // So, we continue dividing n by prime[i] as long as possible
            while (n % prime[i] == 0)
            {
                n /= prime[i]; // we have divided n by prime[i]
                List[listSize] = prime[i]; // added the ith prime in the list
                listSize++; // added a prime, so, size should be increased
            }
        }
    }
}

int main()
{
    primeGeneration();
    primeFactorize(40);
    for (int i = 0; i &lt; listSize; i++) // traverse the List array
        printf(&quot;%d &quot;, List[i]);
}
[/c]

# <span style="color: #ff0000;">**方法二**</span>

方法一并不是那么好，因为如果给定的是一个质数，如19，那么需要检查比它小的所有质数，2、3、5、7、11、13、17。有没有更好的方法呢。其实对于整数n，我们只要检查不大于sqrt(n)的质数就好了。因为任何合数，都会有一个不大于sqrt(n)的质因子。比如对于114这个数，我们检查不超过sqrt(114)=10的质数，先是2，除完变57，再3，除完变19，之后5和7都不能整除，而下一个质数11已经大于10了，所以我们停止检查。而剩下的19，我们知道它肯定没有10以下的质因子了（我们已经都检查过了），那么它肯定是个质数，所以最后我们还要把它加入列表。114=2*3*19。

[c]
#include &lt;cstdio&gt;
#include &lt;cmath&gt;

int N = 10000000;
int status[5000001];
int prime[664579];

void primeGeneration()
{
    for (int i = 1; i &lt;= N&gt;&gt;1; i++) status[i] = 0;

    int sqrtN = sqrt(N);
    for (int i = 3; i &lt;= sqrtN; i += 2)
    {
        if (status[i&gt;&gt;1] == 0)
        {
            for (int j = i*i; j &lt;= N; j += i + i)
                status[j&gt;&gt;1] = 1;
        }
    }

    prime[0] = 2;
    int cnt = 1;
    for (int i = 3; i &lt;= N; i += 2)
        if (status[i&gt;&gt;1]  == 0)
            prime[cnt++] = i;
}

int List[32]; // saves the List
int listSize; // saves the size of List

void primeFactorize(int n)
{
    listSize = 0; // Initially the List is empty, we denote that size = 0
    int sqrtN = sqrt(n);
    for (int i = 0; prime[i] &lt;= sqrtN; i++)
    {
        if (n % prime[i] == 0) // So, n is a multiple of the ith prime
        {
            // So, we continue dividing n by prime[i] as long as possible
            while (n % prime[i] == 0)
            {
                n /= prime[i]; // we have divided n by prime[i]
                List[listSize] = prime[i]; // added the ith prime in the list
                listSize++; // added a prime, so, size should be increased
            }
        }
    }
    if (n &gt; 1)
    {
        List[listSize] = n;
        listSize++;
    }
}

int main()
{
    primeGeneration();
    primeFactorize(114);
    for (int i = 0; i &lt; listSize; i++) // traverse the List array
        printf(&quot;%d &quot;, List[i]);
}
[/c]
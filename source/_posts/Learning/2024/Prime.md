---
title: 质数判断与质数筛
date: 2024-06-14
categories: 
  - Learning
tags: 
  - coding
mathjax: true
description: 刚学C++，如何判断质数？
---



## 什么是质数?

> 质数是指在大于1的自然数中，除了1和它本身以外不再有其他因数的自然数。

> 质数（Prime number），又称素数，指在大于1的自然数中，除了1和该数自身外，无法被其他自然数整除的数（也可定义为只有1与该数本身两个正因数的数）。

可见 **整除** 对于质数的判断有很重要的作用

* * *

进而我们有质数判断的最基本的写法如下↓

## 质数判断 1.0

```cpp
bool Isprime(int n)
{
    if (n == 1) return false;
    for(int i = 2; i < n; i++)
    {
        if(n % i == 0)  return false;
    }
    return true;
}
```

即对于 $2 \leq i \leq n−1$，$i$ 不是 $n$ 的因数

* * *

但是我们还能继续优化这个算法

<!--more-->

## 质数判断 2.0

```cpp
bool Isprime(int n)
{
    if (n == 1) return false;
    double sqrtn = sqrt(n);
    for(int i = 2; i < sqrtn + 1; i++)
    {
        if(n % i == 0)  return false;
    }
    return true;
}
```

即对于 $2≤i≤\sqrt n+1$，$i$ 不是 $n$ 的因数

注意其中的 `double sqrtn = sqrt(n)`语句的位置，这保证了 $\sqrt n$ 不会每次都被计算

* * *

## 质数判断 final 3.0（朴素筛）

朴素筛的时间复杂度是 $O(\sqrt n)$

```cpp
bool Isprime(int n)
{
    if (n == 1)  return false;
    if (n == 2)  return true;
    else if (n % 2 == 0) return false;
    double sqrtn = sqrt(n);
    for(int i = 3; i < sqrtn + 1; i+=2)
    {
        if(n % i == 0)  return false;
    }
    return true;
}
```

即对于 $2≤\sqrt n+1$，奇数 $i$ 不是奇数 $n$ 的因数

其实这里已经能看出来质数筛的思想了，就是逐步利用 **质因子** 筛去合数

* * *

## 质数筛法之埃式筛（埃拉托斯特尼算法）

埃氏筛的时间复杂度是 $O(n log log n)$

要得到自然数 $n$ 以内的全部素数，必须把不大于 $n$ 的所有素数 $p$ 的倍数剔除，剩下的就是素数

例如：使用埃氏筛输出 100 以内的质数

```cpp
#include <iostream>
using namespace std;
const int n = 100;
int main() 
{
    bool R[n + 1];
    for (int i = 0; i <= n; i++) 
        R[i] = true; // 初始化所有数为素数

    for (int i = 2; i * i <= n; i++)
        if (R[i]) 
            for (int j = i * i; j <= n; j += i) 
                R[j] = false; //核心：剔除素数的倍数

    for (int i = 2; i <= n; i++) 
        if (R[i]) 
            cout << i << " "; //输出
    return 0;
}
```

下面，我们审视一下代码

```cpp
for (int i = 2; i * i <= n; i++)
        if (R[i]) 
            for (int j = i * i; j <= n; j += i) 
                R[j] = false; 
```

这里值得注意的地方是 `j = i * i `的初始化

为什么不从 $ki (k<i)$ 开始算起呢？答案是 $ki$ 会被更小的素数筛去，这样做减少了重复筛除

* * *

能不能进一步优化呢？当然是可以的！

## 大名鼎鼎的欧拉筛

欧拉筛的时间复杂度是 $O(n)$

代码如下

```cpp
bool isprime[MAXN]; 
int prime[MAXN]; 
int n;
int cnt; 

void euler()
{
    memset(isprime, true, sizeof(isprime)); 
    isprime[1] = false; 
    for(int i = 2; i <= n; ++i)
    {
        if(isprime[i]) prime[++cnt] = i; // 记录素数
        for(int j = 1; j <= cnt && i * prime[j] <= n; ++j)
        {
            isprime[i * prime[j]] = false;  // 筛去i的质数倍
            if(i % prime[j] == 0) break; //直到遇到i的最小质因子
        }
    }
}
```

下面，我们审视一下这个代码的关键之处

```cpp
for(int j = 1; j <= cnt && i * prime[j] <= n; ++j)
{
    isprime[i * prime[j]] = false;  //筛去i的质数倍
    if(i % prime[j] == 0) break; //直到遇到最小质因子
}
```

对于任意合数 $N=pX$ (其中 $p$ 为 $N$ 的最小质因子)  
那么，由于 $X<N$, $X$ 会先进入筛子

关键：$X$ 的最小质因子不会小于 $p$，所以 $N$ 会被它的最小质因子 $p$ 筛去
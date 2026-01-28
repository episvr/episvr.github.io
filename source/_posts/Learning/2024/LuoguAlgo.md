---
title:  题解：[NOIP1998 提高组] 车站
date: 2024-08-24
categories: 
  - Learning
tags: 
  - coding
mathjax: true
description: 认真分析就能避免爆破
---

> ###  题目描述
>
> 火车从始发站（称为第 $1$ 站）开出，在始发站上车的人数为 $a$，然后到达第 $2$ 站，在第 $2$ 站有人上、下车，但上、下车的人数相同，因此在第 $2$ 站开出时（即在到达第 $3$ 站之前）车上的人数保持为 $a$ 人。从第 $3$ 站起（包括第 $3$ 站）上、下车的人数有一定规律：上车的人数都是前两站上车人数之和，而下车人数等于上一站上车人数，一直到终点站的前一站（第 $n-1$ 站），都满足此规律。现给出的条件是：共有 $n$ 个车站，始发站上车的人数为 $a$，最后一站下车的人数是 $m$（全部下车）。试问 $x$ 站开出时车上的人数是多少

这说的也太乱了，读着不是很友好，我们先整理一下，假设`up[],dn[],tar[]`三个数组，有：

```
up[1] = tar[1] = tar[2] = a
up[2] = dn[2]
dn[n] = tar[n-1] = m
tar[k] = tar[k-1] + up[k] - dn[k]
//if k>=3 && k<=n-1:
	up[k] = up[k-1] + up[k-2]
	dn[k] = up[k-1]
```

由于数据量不大，所以完全能爆了，但由于Epi上午刚给一个小盆友复习数列，于是就有了如下过程。

通过观察式子，我们能注意到几件事情：

1. `dn[]`完全就是`up[]`的一部分，没有独立出来的必要

   把`tar[k] = tar[k-1] + up[k] - dn[k]`中的`dn[k]`加以替换，即可得到：

   `tar[k] = tar[k-1] + up[k] - up[k-1]`

   进而可知`tar[k] - up[k]`是一个**常数列**（其中 2 < k < n - 1）注意是从 k = 2 开始

2. `up[]`的递推形式很像**斐波那契数列**

3. 结合1，2的信息，可知`up[2]`是关键数，一旦得知即可推出数列全部信息

不妨先假设`up[2] = y`，则可以写出`up[]`数组对应的数列$\{b_n\}$：
$$
\begin{flalign}
b_1&=a \\\\
b_2&=y \\\\
b_3&= a+y \\\\
b_4&= a+2y \\\\
b_5&=2a+3y \\\\
b_6&=3a+5y \\\\
b_7&=5a+8y&
\end{flalign}
$$
欸？有斐波那契的影子！观察 $b_n$ 的生成规律就会发现这不是一个巧合

于是可以得知：

$b_n = F_{n-2}a+F_{n-1}y$

由于`tar[n-1] = m` 及 `tar[n-1] - up[n-1] = tar[2] - up[2]`  可得

$m-(F_{n-3}a+F_{n-2}y) = a - y$

解出$y$，很快就可以求出数列的所有信息了（好耶！



过关代码如下：~~Epi没有做特殊判断好孩子千万不要学他~~

```cpp
#include <iostream>
using namespace std;
int a,n,m,x;
//int people[20],up[20];
int magicnum,ans;
int fib[25]={0,1};
int main()
{
	cin>>a>>n>>m>>x;
	for(int i=2;i<25;i++)
		fib[i]=fib[i-1]+fib[i-2];
    //up[k]=up[k-1]+up[k-2] fibonacci up[1]=a,up[2]= y
	//people[k] = a-y+up[k] is a const num; k>=2
	//people[n-1] = a-y+up[k-1] = m;
	magicnum = (m-(fib[n-3]+1)*a)/(fib[n-2]-1);
	ans = (fib[x-2]+1)*a+(fib[x-1]-1)*magicnum; 
	cout<< ans;
}
```

好的，你说的对，但我选择爆破。

此处应有：我以后天天笑.jpg

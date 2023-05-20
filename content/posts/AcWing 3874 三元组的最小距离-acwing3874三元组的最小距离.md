---
title: AcWing 3874. 三元组的最小距离
date: 2022-08-19 08:14:15.209
updated: 2022-08-19 08:47:58.029
url: /p=59
categories: 
tags: 
- AcWing
- 考研算法
---

## 题目
定义三元组 $(a,b,c)$（$a$,$b$,$c$ 均为整数）的距离 $D=|a−b|+|b−c|+|c−a|$。

给定 3 个非空整数集合 $S_1,S_2,S_3$，按**升序**分别存储在 $3$ 个数组中。

请设计一个尽可能高效的算法，计算并输出所有可能的三元组 $(a,b,c)（a∈S1,b∈S2,c∈S3）$中的最小距离。

例如 $S_1=\{−1,0,9\},S_2=\{−25,−10,10,11\},S_3=\{2,9,17,30,41\}$ 则最小距离为 $2$，相应的三元组为 $(9,10,9)$。

**输入格式**
第一行包含三个整数 $l$,$m$,$n$，分别表示 $S_1,S_2,S_3$ 的长度。

第二行包含 $l$ 个整数，表示 $S_1$ 中的所有元素。

第三行包含 $m$ 个整数，表示 $S_2$ 中的所有元素。

第四行包含 $n$ 个整数，表示 $S_3$ 中的所有元素。

以上三个数组中的元素都是按升序顺序给出的。

**输出格式**
输出三元组的最小距离。

**数据范围**
$1≤l,m,n≤10^5$,
所有数组元素的取值范围 $[−10^9,10^9]$。

**输入样例：**
```
3 4 5
-1 0 9
-25 -10 10 11
2 9 17 30 41
```

**输出样例：**
```
2
```

## 思路
![image-1660898848923](upload/2022/08/image-1660898848923.png)

## 题解 - yxc版
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 100010;

int l, m, n;
int a[N], b[N], c[N];

int main()
{
    scanf("%d%d%d", &l, &m, &n);
    for (int i = 0; i < l; i ++ ) scanf("%d", &a[i]);
    for (int i = 0; i < m; i ++ ) scanf("%d", &b[i]);
    for (int i = 0; i < n; i ++ ) scanf("%d", &c[i]);

    LL res = 1e18;
    for (int i = 0, j = 0, k = 0; i < l && j < m && k < n;)
    {
        int x = a[i], y = b[j], z = c[k];
        res = min(res, (LL)max(max(x, y), z) - min(min(x, y), z));
        if (x <= y && x <= z) i ++ ;
        else if (y <= x && y <= z) j ++ ;
        else k ++ ;
    }

    printf("%lld\n", res * 2);
    return 0;
}
```

## 参考链接
https://www.acwing.com/solution/content/66478/
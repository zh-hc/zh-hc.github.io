---
title: AcWing 854. Floyd求最短路
date: 2022-08-09 07:03:42.97
updated: 2022-08-09 07:26:11.732
url: /p=53
categories: 
tags: 
- AcWing
- 考研算法
---

## 题目
给定一个 $n$ 个点 $m$ 条边的有向图，图中可能存在重边和自环，边权可能为负数。

再给定 $k$ 个询问，每个询问包含两个整数 $x$ 和 $y$，表示查询从点 $x$ 到点 $y$ 的最短距离，如果路径不存在，则输出 `impossible`。

数据保证图中不存在负权回路。

**输入格式**
第一行包含三个整数 $n$,$m$,$k$。

接下来 $m$ 行，每行包含三个整数 $x$,$y$,$z$，表示存在一条从点 $x$ 到点 $y$ 的有向边，边长为 $z$。

接下来 $k$ 行，每行包含两个整数 $x$,$y$，表示询问点 $x$ 到点 $y$ 的最短距离。

**输出格式**
共 $k$ 行，每行输出一个整数，表示询问的结果，若询问两点间不存在路径，则输出 `impossible`。

**数据范围**
$1≤n≤200$,
$1≤k≤n^2$
$1≤m≤20000$,
图中涉及边长绝对值均不超过 $10000$。

**输入样例：**
```
3 3 2
1 2 1
2 3 2
1 3 1
2 1
1 3
```

**输出样例：**
```
impossible
1
```

## 思路
`Floyd` 算法 - 多源汇最短路 - 具有多个源点 - 时间复杂度 $O(n^3)$
只需要进行一个三重循环
原理基于动态规划（状态表示是三维 $d[k,i,j]$）
$d[k,i,j]$ 表示从 $i$ 这个点只经过 $1 \sim k$ 这些中间点最后到达 $j$ 的最短距离
$d[k,i,j]=d[k-1,i,k]+d[k-1,k,j]$
用`邻接矩阵`来存储所有的边

## 题解 - yxc版本
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 210, INF = 0x3f3f3f3f;

int n, m, Q;
int d[N][N];

int main()
{
    scanf("%d%d%d", &n, &m, &Q);
    memset(d, 0x3f, sizeof d);
    for (int i = 1; i <= n; i ++ ) d[i][i] = 0;

    while (m -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        d[a][b] = min(d[a][b], c);
    }

    for (int k = 1; k <= n; k ++ )
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= n; j ++ )
                d[i][j] = min(d[i][j], d[i][k] + d[k][j]);

    while (Q -- )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        int c = d[a][b];
        if (c > INF / 2) puts("impossible");
        else printf("%d\n", c);
    }

    return 0;
}
```
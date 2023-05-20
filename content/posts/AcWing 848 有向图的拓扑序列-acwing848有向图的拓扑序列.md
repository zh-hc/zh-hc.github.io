---
title: AcWing 848. 有向图的拓扑序列
date: 2022-08-06 02:35:36.219
updated: 2022-08-09 06:18:14.453
url: /p=49
categories: 
tags: 
- AcWing
- 考研算法
---

## 题目
给定一个 $n$ 个点 $m$ 条边的有向图，点的编号是 $1$ 到 $n$，图中可能存在重边和自环。
请输出任意一个该有向图的拓扑序列，如果拓扑序列不存在，则输出 $−1$。
若一个由图中所有点构成的序列 $A$ 满足：对于图中的每条边 $(x,y)$，$x$ 在 $A$ 中都出现在 $y$ 之前，则称 $A$ 是该图的一个拓扑序列。

**输入格式**
第一行包含两个整数 $n$ 和 $m$。
接下来 $m$ 行，每行包含两个整数 $x$ 和 $y$，表示存在一条从点 $x$ 到点 $y$ 的有向边 $(x,y)$。

**输出格式**
共一行，如果存在拓扑序列，则输出任意一个合法的拓扑序列即可。
否则输出 $−1$。

**数据范围**
$1≤n$,$m≤10^5$

**输入样例：**
```
3 3
1 2
2 3
1 3
```

**输出样例：**
```
1 2 3
```

## 思路
**一些定义**
`入度`(in-degree) ：以某顶点为弧头，终止于该顶点的弧的数目称为该顶点的入度
`出度`(out-degree) ：以某顶点为弧尾，起始于该顶点的弧的数目称为该顶点的出度

**两种方法**：宽搜(BFS)、深搜(DFS)
一般使用`宽搜`，因为有一些编译器没有把栈空间放开，而导致我们在使用深搜方式时无法正常编译。
我们可以知道，题目中的`存在拓扑排序`即等价于`无环`。

**宽搜(BFS)**
> BFS算法概览：先将度数为`0`的点加入队列，然后遍历这些点分别进行`BFS`，搜索到的点度数减`1`，度数为`0`时加入队列。

1. 首先找到第一个入度为 `0` 的点 放入待处理队列，记录在拓扑数组中
2. 然后针对`该点连接的各个点`做以下操作：
	- 2.1 删除该边后，查看从该点连接的的点的入度
	- 2.2 如果入度为 0，那么该点放入待处理队列，记录在拓扑数组中， 再次进行BFS，直到待处理队列为空

**深搜(DFS)**
DFS就是`一直沿着一个路径`走，直到某个顶点`没有出度`之后再往回走
该算法通过是否重复搜到递归搜索中的结点来`判断图是否无环`
最后队列保存的序列是拓扑排序的`逆序`，需要逆序输出。

**大佬笔记**
![image-1659757664305](upload/2022/08/image-1659757664305.png)

## 题解 - yxc
**基于宽搜的拓扑排序**
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 100010, M = 100010;

int n, m;
struct Node
{
    int id;
    Node* next;
    Node(int _id): id(_id), next(NULL) {}
}*head[N];
int d[N], q[N];

void add(int a, int b)
{
    auto p = new Node(b);
    p->next = head[a];
    head[a] = p;
}

bool topsort()
{
    int hh = 0, tt = -1;
    for (int i = 1; i <= n; i ++ )
        if (!d[i])
            q[ ++ tt] = i;

    while (hh <= tt)
    {
        int t = q[hh ++ ];
        for (auto p = head[t]; p; p = p->next)
            if ( -- d[p->id] == 0)
                q[ ++ tt] = p->id;
    }

    return tt == n - 1;
}

int main()
{
    scanf("%d%d", &n, &m);
    while (m -- )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        d[b] ++ ;
        add(a, b);
    }

    if (!topsort()) puts("-1");
    else
    {
        for (int i = 0; i < n; i ++ )
            printf("%d ", q[i]);
    }

    return 0;
}
```

**基于深搜的拓扑排序**
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 100010, M = 100010;

int n, m;
struct Node
{
    int id;
    Node* next;
    Node(int _id): id(_id), next(NULL) {}
}*head[N];
int st[N], q[N], top;

void add(int a, int b)
{
    auto p = new Node(b);
    p->next = head[a];
    head[a] = p;
}

bool dfs(int u)
{
    st[u] = 1;

    for (auto p = head[u]; p; p = p->next)
    {
        int j = p->id;
        if (!st[j])
        {
            if (!dfs(j)) return false;
        }
        else if (st[j] == 1) return false;
    }

    q[top ++ ] = u;

    st[u] = 2;
    return true;
}

bool topsort()
{
    for (int i = 1; i <= n; i ++ )
        if (!st[i] && !dfs(i))
            return false;
    return true;
}

int main()
{
    scanf("%d%d", &n, &m);
    while (m -- )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        add(a, b);
    }

    if (!topsort()) puts("-1");
    else
    {
        for (int i = n - 1; i >= 0; i -- )
            printf("%d ", q[i]);
    }

    return 0;
}
```
---
title: AcWing 831. KMP字符串
date: 2022-08-14 02:03:06.391
updated: 2022-08-14 08:07:52.879
url: /p=56
categories: 
tags: 
- AcWing
- 考研算法
---

## 题目
给定一个字符串 $S$，以及一个模式串 $P$，所有字符串中只包含大小写英文字母以及阿拉伯数字。

模式串 $P$ 在字符串 $S$ 中多次作为子串出现。

求出模式串 $P$ 在字符串 $S$ 中所有出现的位置的起始下标。

**输入格式**
第一行输入整数 $N$，表示字符串 $P$ 的长度。

第二行输入字符串 $P$。

第三行输入整数 $M$，表示字符串 $S$ 的长度。

第四行输入字符串 $S$。

**输出格式**
共一行，输出所有出现位置的起始下标（下标从 $0$ 开始计数），整数之间用空格隔开。

**数据范围**
$1≤N≤10^5$
$1≤M≤10^6$

**输入样例：**
```
3
aba
5
ababa
```

**输出样例：**
```
0 2
```

## 思路
定义一个 $next$ 数组，其中 $next[i]$ 表示 `前 i 个字母构成的字符串中最长的与前缀相等的后缀的长度`
以 `abaabc` 为例求 $next[5]$，也就是前五个字符 `abaab`，可以看到，取前 `4` 位时 `abaa≠baab`，取前 `3` 位时 `aba≠aab`，取前 `2` 位时 `ab=ab`，所以可以知道 $next[5]=2$
有了这个我们就可以在匹配不成功时直接跳到下一次前缀正确的位置继续进行匹配了

## 题解
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 100010, M = 1000010;

int n, m;
char p[N], s[M];
int ne[N];

int main()
{
    scanf("%d%s", &n, p + 1);
    scanf("%d%s", &m, s + 1);

    for (int i = 2, j = 0; i <= n; i ++ )
    {
        while (j && p[i] != p[j + 1]) j = ne[j];
        if (p[i] == p[j + 1]) j ++ ;
        ne[i] = j;
    }

    for (int i = 1, j = 0; i <= m; i ++ )
    {
        while (j && s[i] != p[j + 1]) j = ne[j];
        if (s[i] == p[j + 1]) j ++ ;
        if (j == n) printf("%d ", i - n);
    }
    return 0;
}
```
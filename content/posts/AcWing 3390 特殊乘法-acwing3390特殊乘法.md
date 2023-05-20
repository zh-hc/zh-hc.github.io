---
title: AcWing 3390. 特殊乘法
date: 2022-09-21 02:32:35.841
updated: 2022-09-21 06:23:03.207
url: /p=64
categories: 
tags: 
- AcWing
- 考研算法
---

## 题目
给定一个 $n$ 位整数 $A$，各位从高到低依次为 $a_1,a_2,\dots ,a_n$。

给定一个 $m$ 位整数 $B$，各位从高到低依次位 $b_1,b_2,\dots ,b_m$。

给定一种特殊乘法，不妨用 $\otimes$ 来表示，我们规定 $A \otimes B=\sum\limits _{i=1}^n \sum \limits_{j=1}^m a_i \times b_j$
例如，$123 \otimes 45=1×4+1×5+2×4+2×5+3×4+3×5$。

对于给定的 $A$ 和 $B$，请你计算并输出 $A \otimes B$ 的值。

**输入格式**
两个整数 $A$ 和 $B$。

**输出格式**
一个整数，表示 $A \otimes B$ 的值。

**数据范围**
$1≤A,B≤10^9$

**输入样例：**
```
123 45
```

**输出样例：**
```
54
```

## 题解 - yxc版本
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

int main()
{
    string a, b;
    cin >> a >> b;
    int res = 0;
    for (auto x: a)
        for (auto y: b)
            res += (x - '0') * (y - '0');
    cout << res << endl;
    return 0;
}
```
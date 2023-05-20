---
title: AcWing 3472. 八皇后
date: 2022-09-02 03:11:11.978
updated: 2022-09-02 05:53:50.358
url: /p=62
categories: 
tags: 
- AcWing
- 考研算法
---

## 题目
会下国际象棋的人都很清楚：皇后可以在横、竖、斜线上不限步数地吃掉其他棋子。

如何将 $8$ 个皇后放在棋盘上（有 $8×8$ 个方格），使它们谁也不能被吃掉！

这就是著名的八皇后问题。

对于某个满足要求的 $8$ 皇后的摆放方法，定义一个皇后串 $a$ 与之对应，即 $a=b_1b_2…b_8$，其中 $b_i$ 为相应摆法中第 $i$ 行皇后所处的列数。

已经知道 $8$ 皇后问题一共有 $92$ 组解（即 $92$ 个不同的皇后串）。

给出一个数 $b$，要求输出第 $b$ 个串。

串的比较是这样的：皇后串 $x$ 置于皇后串 $y$ 之前，当且仅当将 $x$ 视为整数时比 $y$ 小。

**输入格式**
第一行包含整数 $n$，表示共有 $n$ 组测试数据。

每组测试数据占 $1$ 行，包括一个正整数 $b$。

**输出格式**
输出有 $n$ 行，每行输出对应一个输入。

输出应是一个正整数，是对应于 $b$ 的皇后串。

**数据范围**
$1≤b≤92$

**输入样例：**
```
2
1
92
```

**输出样例：**
```
15863724
84136275
```

## 思路
**如何判断某条对角线被用过**
正对角线：$2n-1$ 条
反对角线：$2n-1$ 条
可以用 $y=x+b(即b=y-x)$ 和 $y=-x+b(即b=y+x)$ 分别表示两条对角线进行映射(编号)
而 $y-x$ 可能为负数，为了防止出现负数的情况，我们变换为 $y-x+n$ 即可。

**思路**
8皇后的解题就是一个试错的过程：
我们将整个 $8×8$ 的棋盘按照行的顺序从上往下选，每一行中选择一个位置，如果选到了第 $i$ 行的第 $j$ 列发现，皇后们冲突了，我们就尝试将当前行的皇后移动到第 $j+1$ 列，直到选择到最后一行都没有冲突，我们则认为出现了合法方案。
如果对于某一行我们选择到了最后的一个位置都不能避免冲突，那么我们就要回溯到上一行去改变上一行的皇后所在列的位置。

## 题解 - yxc版本
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 20;

int n;
bool col[N], dg[N], udg[N];
int path[N];

bool dfs(int u)
{
    if (u == 8)
    {
        if ( -- n == 0)
        {
            for (int i = 0; i < 8; i ++ )
                cout << path[i];
            cout << endl;
            return true;
        }
        return false;
    }

    for (int i = 0; i < 8; i ++ )
        if (!col[i] && !dg[u + i] && !udg[u - i + 8])
        {
            col[i] = dg[u + i] = udg[u - i + 8] = true;
            path[u] = i + 1;
            if (dfs(u + 1)) return true;
            col[i] = dg[u + i] = udg[u - i + 8] = false;
        }
    return false;
}

int main()
{
    int T;
    cin >> T;
    while (T -- )
    {
        cin >> n;
        memset(col, 0, sizeof col);
        memset(dg, 0, sizeof dg);
        memset(udg, 0, sizeof udg);
        dfs(0);
    }
    return 0;
}
```
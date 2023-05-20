---
title: AcWing 3429. 全排列
date: 2022-08-26 10:47:30.076
updated: 2022-08-26 11:03:56.181
url: /p=61
categories: 
tags: 
- AcWing
- 考研算法
---

## 题目
给定一个由不同的小写字母组成的字符串，输出这个字符串的所有全排列。

我们假设对于小写字母有 $a<b<…<y<z$，而且给定的字符串中的字母已经按照从小到大的顺序排列。

**输入格式**
输入只有一行，是一个由不同的小写字母组成的字符串，已知字符串的长度在 $1$ 到 $6$ 之间。

**输出格式**
输出这个字符串的所有排列方式，每行一个排列。

要求字母序比较小的排列在前面。

字母序如下定义：

已知 $S=s_1s_2…s_k,T=t_1t_2…t_k$，则 $S<T$ 等价于，存在 $p(1≤p≤k)$，使得 $s_1=t_1,s_2=t_2,…,s_{p−1}=t_{p−1},s_p<t_p$ 成立。

**数据范围**
字符串的长度在 $1$ 到 $6$ 之间

**输入样例：**
```
abc
```

**输出样例：**
```
abc
acb
bac
bca
cab
cba
```

## 思路
**递归搜索树**
```cpp
void dfs(int u)
{
    //当已经填满了前n个位置,将path数组输出
    if(u == n)
    {
        for(int i = 0; i < n; i ++) cout << path[i];
        cout << endl;
        return;
    }

    //枚举每一个字符的位置
    for(int i = 0; i < n; i ++)
    {
        if(!st[i])             //当这个第i个字符还没有使用过
        {
            path[u] = s[i];   //加入path数组中
            st[i] = true;     //标记为已经使用
            dfs(u + 1);       //去填写path数组的下一个位置
            st[i] = false;    //恢复现场
        }
    }
}
```

## 题解 - yxc版本
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 10;

int n;
char str[N], path[N];
bool st[N];

void dfs(int u)
{
    if (u == n) cout << path << endl;
    else
    {
        for (int i = 0; i < n; i ++ )
            if (!st[i])
            {
                path[u] = str[i];
                st[i] = true;
                dfs(u + 1);
                st[i] = false;
            }
    }
}

int main()
{
    cin >> str;
    n = strlen(str);
    dfs(0);
    return 0;
}
```
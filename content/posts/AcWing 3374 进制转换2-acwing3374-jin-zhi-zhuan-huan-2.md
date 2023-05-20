---
title: AcWing 3374. 进制转换2
date: 2022-07-17 15:20:14.0
updated: 2022-08-02 02:32:23.215
url: /p=16
categories: 
tags: 
- AcWing
- 考研算法
---

**高精度**

## 题目
将 $M$ 进制的数 $X$ 转换为 $N$ 进制的数输出。

**输入格式**
第一行包括两个整数：$M$ 和 $N$。
第二行包含一个数 $X$，$X$ 是 $M$ 进制的数，现在要求你将 $M$ 进制的数 $X$ 转换成 $N$ 进制的数输出。

**输出格式**
共一行，输出 $X$ 的 $N$ 进制表示。

**数据范围**
$2≤N$,$M≤36$,
$X$ 最多包含 $100$ 位。
在输入中，当某一位数字的值大于 $10$（十进制下）时，我们用大写字母 $A∼Z$，分别表示（十进制下的）数值 $10∼35$。
在输出中，当某一位数字的值大于 $10$（十进制下）时，我们用小写字母 $a∼z$，分别表示（十进制下的）数值 $10∼35$。

**输入样例：**
```
10 2
11
```

**输出样例：**
```
1011
```

## 知识点
**短除法实现**
```cpp
// A的长度不是0的时候
while (A.size())
{
    int r = 0;
    for (int i = A.size() - 1; i >= 0; i -- )
    {
        // 商
        A[i] += r * a;
        // r保存余数
        r = A[i] % b;
        // 商
        A[i] /= b;
    }
    // 删除前导0
    while (A.size() && A.back() == 0) A.pop_back();
    if (r < 10) res += to_string(r);
    else res += r - 10 + 'a';
}
```

## 题解
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
#include <vector>

using namespace std;

int main()
{
    int a, b;
    string s;
    cin >> a >> b >> s;
    vector<int> A;
    for (int i = 0; i < s.size(); i ++ )
    {
        char c = s[s.size() - 1 - i];
        if (c >= 'A') A.push_back(c - 'A' + 10);
        else A.push_back(c - '0');
    }
    string res;
    if (s == "0") res = "0";
    else
    {
        while (A.size())
        {
            int r = 0;
            for (int i = A.size() - 1; i >= 0; i -- )
            {
                A[i] += r * a;
                r = A[i] % b;
                A[i] /= b;
            }
            while (A.size() && A.back() == 0) A.pop_back();
            if (r < 10) res += to_string(r);
            else res += r - 10 + 'a';
        }
        reverse(res.begin(), res.end());
    }
    cout << res << endl;

    return 0;
}
```
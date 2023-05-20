---
title: AcWing 3379. 反序输出
date: 2022-09-04 02:37:58.232
updated: 2022-09-04 02:42:01.835
url: /p=63
categories: 
tags: 
- AcWing
- 考研算法
---

## 题目
输入任意 $4$ 个字符(如：`abcd`)， 并按反序输出(如：`dcba`)。

**输入格式**
输入包含多组测试数据。

每组数据占一行，包含四个字符（大小写字母）。

**输出格式**
对于每组输入，输出一行反序后的字符串。

**数据范围**
输入最多包含 $100$ 组数据。

**输入样例：**
```
Upin
cvYj
WJpw
cXOA
```

**输出样例：**
```
nipU
jYvc
wpJW
AOXc
```

## 思路
将输入反序输出即可。

## 题解 - yxc版本
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

int main()
{
    string s;
    while (cin >> s)
    {
        reverse(s.begin(), s.end());
        cout << s << endl;
    }
    return 0;
}
```
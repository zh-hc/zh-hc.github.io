---
title: AcWing 3607. 打印日期
date: 2022-07-22 03:40:58.997
updated: 2022-08-03 05:21:29.974
url: /p=34
categories: 
tags: 
- AcWing
- 考研算法
---

## 题目
给出年份 $y$ 和一年中的第 $d$ 天，算出第 $d$ 天是几月几号。

**输入格式**
输入包含多组测试数据。
每组数据占一行，包含两个整数 $y$ 和 $d$。

**输出格式**
每组数据输出一行一个结果，格式为`yyyy-mm-dd`。

**数据范围**
输入最多包含 $100$ 组数据,
$1≤y≤3000$,
$1≤d≤366$,
数据保证合法。

**输入样例：**
```
2000 3
2000 31
2000 40
2000 60
2000 61
2001 60
```

**输出样例：**
```
2000-01-03
2000-01-31
2000-02-09
2000-02-29
2000-03-01
2001-03-01
```

## 思路
> 遇到有关日期的问题，先将如下三个函数写上，基本即可解决全部问题。

**1. 求每个月的天数的函数**
```cpp
// 二月先记为平年的情况下，每月的天数。
const int months[13] = {
    0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31
};
```

**2. 判断闰年的函数**
```cpp
// 闰年返回1，平年返回0
int is_leap(int year) {
    if (year % 4 == 0 && year % 100 || year % 400 == 0)
        return 1;
    return 0;
}
```

**3. 求 $y$ 年 $m$ 月有多少天的函数**
```cpp
// 返回y年m月有多少天
int get_days(int y, int m) {
    if (m == 2) return months[m] + is_leap(y);
    return months[m];
}
```

## 题解
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int months[13] = {
    0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31
};

int is_leap(int year)  // 闰年返回1，平年返回0
{
    if (year % 4 == 0 && year % 100 || year % 400 == 0)
        return 1;
    return 0;
}

int get_days(int y, int m)  // y年m月有多少天
{
    if (m == 2) return months[m] + is_leap(y);
    return months[m];
}

int main()
{
    int y, s;

    while (cin >> y >> s)
    {
        int m = 1, d = 1;
        s -- ;
        while (s -- )
        {
            if ( ++ d > get_days(y, m))
            {
                d = 1;
                if ( ++ m > 12)
                {
                    m = 1;
                    y ++ ;
                }
            }
        }
        printf("%04d-%02d-%02d\n", y, m, d);
    }
    return 0;
}
```
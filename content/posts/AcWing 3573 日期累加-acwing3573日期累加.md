---
title: AcWing 3573. 日期累加
date: 2022-07-23 04:02:30.654
updated: 2022-08-03 05:24:23.663
url: /p=35
categories: 
tags: 
- AcWing
- 考研算法
---

## 题目
设计一个程序能计算一个日期加上若干天后是什么日期。

**输入格式**
第一行包含整数 $T$，表示共有 $T$ 组测试数据。
每组数据占一行，包含四个整数 $y$,$m$,$d$,$a$，分别表示给定日期的年、月、日和累加的天数。

**输出格式**
每组数据输出一行，一个结果，每行按`yyyy-mm-dd`的格式输出。

**数据范围**
$1≤T≤1000$
$1000≤y≤3000$,
$1≤m≤12$,
$1≤d≤31$,
$1≤a≤10^6$,
保证输入日期合法。

**输入样例：**
```
1
2008 2 3 100
```

**输出样例：**
```
2008-05-13
```

## 思路
**优化**
每次枚举天数是$10^6$次，本次测试数据有$1000$个，总次数为$10^9$
而指导思想中我们需要控制到 $10^7$ ~ $10^8$，超过了时间限制，因此我们需要用到`优化`。
可以先一年一年数(最多约为 $10^6/365≈2740$ 次)
再一天一天数(`365`次)
1000个输入大约共 $3000 * 1000=3000000$ 次，符合要求

**先写上时间相关的常用代码**
```cpp
const int months[13] = {
    0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31
};

int is_leap(int year)
{
    if (year % 4 == 0 && year % 100 || year % 400 == 0)
        return 1;
    return 0;
}

int get_days(int y, int m)
{
    if (m == 2) return months[m] + is_leap(y);
    return months[m];
}
```

**对2月29日进行特判**
减少后续代码对于闰年的判断
```cpp
if (m == 2 && d == 29) a --, m = 3, d = 1;
```

**一年一年数**
get_year_days函数用于返回当年的天数，在下文有实现
```cpp
while (a > get_year_days(y, m))
{
    a -= get_year_days(y, m);
    y ++ ;
}
```

**一天一天数**
```cpp
while (a -- )
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
```

**返回当年的天数**
1月1日~2月28日：判断`当年`是否为闰年
3月1日~12月31日：判断`明年`是否为闰年
```cpp
int get_year_days(int y, int m)
{
    if (m <= 2) return 365 + is_leap(y);
    return 365 + is_leap(y + 1);
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

int is_leap(int year)
{
    if (year % 4 == 0 && year % 100 || year % 400 == 0)
        return 1;
    return 0;
}

int get_days(int y, int m)
{
    if (m == 2) return months[m] + is_leap(y);
    return months[m];
}

int get_year_days(int y, int m)
{
    if (m <= 2) return 365 + is_leap(y);
    return 365 + is_leap(y + 1);
}

int main()
{
    int T;
    cin >> T;
    while (T -- )
    {
        int y, m, d, a;
        cin >> y >> m >> d >> a;
        if (m == 2 && d == 29) a --, m = 3, d = 1;
        while (a > get_year_days(y, m))
        {
            a -= get_year_days(y, m);
            y ++ ;
        }
        while (a -- )
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
---
title: AcWing 3373. 进制转换
date: 2022-07-16 15:19:13.0
updated: 2022-08-02 02:30:15.055
url: /p=14
categories: 
tags: 
- AcWing
- 考研算法
---

## 题目
将一个长度最多为 $30$ 位数字的十进制非负整数转换为二进制数输出。

**输入格式**
输入包含多组测试数据。
每组测试数据占一行，包含一个长度不超过 $30$ 位的十进制非负整数。

**输出格式**
每组数据输出一个结果，占一行，为输入对应的二进制数。

**数据范围**
输入最多包含 $100$ 组测试数据。

**输入样例：**
```
0
1
3
8
```

**输出样例：**
```
0
1
11
1000
```

## 知识点
**高精度**
由题目可得，数字位数为`30位`，这个长度超过了long long范围，所以我们需要借助数组来存储
也就是使用数组来`模拟`数据，比如将五位数 $12345$ 的每一位存在数组a中：
我们从个位开始存(即倒着存)即：`a[0]=5; a[1]=4; a[2]=3; a[3]=2; a[4]=1`

**如何计算除法**
通过`模拟`列竖式的方式，即从最高位开始算除法(在本题中为除以 $2$ )
1. 将除法得到的商push到结果 $C$ 中
2. 将除法得到的余数和下一位构成一个新的数，继续进行除以 $2$ 的运算，直到把被除数的每一位遍历完成为止。

**将字符变成数字**
`s[s.size() - i - 1] - '0'`

**特殊值判断**
如果输入值为 $0$，那么结果为 $0$
`if (s == "0") res = "0";`

**短除法部分**
```cpp
// 当A的长度不是0的时候
while (A.size())
{
    // 个位除以2的余数
    res += to_string(A[0] % 2);
    // A / 2
    A = div(A, 2);
}
```

**div函数实现**
```cpp
vector<int> div(vector<int> A, int b)
{
    // 将结果定义为C
    vector<int> C;
    // 从最高位开始循环做除法
    for (int i = A.size() - 1, r = 0; i >= 0; i -- )
    {
        // 使用r来保存余数与下一位组成的新数 初值为0
        r = r * 10 + A[i];
        // 使用C来保存新数与b相除得到的商
        C.push_back(r / b);
        // 保存本轮计算后的余数
        r %= b;
    }
    // 将C逆序
    reverse(C.begin(), C.end());
    // 删除掉前导0
    while (C.size() && C.back() == 0) C.pop_back();
    return C;
}
```

## 题解
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
#include <vector>

using namespace std;

vector<int> div(vector<int> A, int b)
{
    vector<int> C;
    for (int i = A.size() - 1, r = 0; i >= 0; i -- )
    {
        r = r * 10 + A[i];
        C.push_back(r / b);
        r %= b;
    }
    reverse(C.begin(), C.end());
    while (C.size() && C.back() == 0) C.pop_back();
    return C;
}

int main()
{
    string s;
    while (cin >> s)
    {
        vector<int> A;
        for (int i = 0; i < s.size(); i ++ )
            A.push_back(s[s.size() - i - 1] - '0');

        string res;
        if (s == "0") res = "0";
        else
        {
            while (A.size())
            {
                res += to_string(A[0] % 2);
                A = div(A, 2);
            }
        }
        reverse(res.begin(), res.end());
        cout << res << endl;
    }

    return 0;
}
```
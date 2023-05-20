---
title: AcWing 3302. 表达式求值
date: 2022-07-24 15:54:38.502
updated: 2022-08-02 03:47:54.513
url: /p=37
categories: 
tags: 
- AcWing
- 考研算法
---

## 题目
给定一个表达式，其中运算符仅包含`+,-,*,/`（加 减 乘 整除），可能包含括号，请你求出表达式的最终值。

**注意：**
数据保证给定的表达式合法。
题目保证符号`-`只作为减号出现，不会作为负号出现，例如，`-1+2`,`(2+2)*(-(1+1)+2)`之类表达式均不会出现。
题目保证表达式中所有数字均为正整数。
题目保证表达式在中间计算过程以及结果中，均不超过$2^{31}−1$。
题目中的整除是指向 $0$ 取整，也就是说对于大于 $0$ 的结果向下取整，例如 $5/3=1$，对于小于 $0$ 的结果向上取整，例如 $5/(1−4)=−1$。
C++和Java中的整除默认是向零取整；Python中的整除`//`默认向下取整，因此Python的`eval()`函数中的整除也是向下取整，在本题中不能直接使用。

**输入格式**
共一行，为给定表达式。

**输出格式**
共一行，为表达式的结果。

**数据范围**
表达式的长度不超过 $10^5$。

**输入样例：**
```
(2+2)*(1+1)
```

**输出样例：**
```
8
```

## 思路及注意事项
**注意**
本题需要背过，是一个典型的模版，包括中缀表达式转后缀表达式的代码
> 先记忆思路，后背代码

**模板思路**
1. 使用两个栈
	1. 第一个栈`(运算符栈)`存加减乘除这些运算符号
	2. 第二个栈`(数栈)`存数字
2.  遍历输入的表达式，遇到对应的字符处理如下：
    - `数`
    	- 压入`数栈`
    - `左括号`
    	- 压入`运算符栈`
    - `右括号`
    	- `操作`到遇到左括号为止
    - `加、减、乘、除`(除括号外的运算符号)
    	- `操作`到遇到左括号或者`栈顶运算符`优先级**小于**`当前运算符`优先级为止
3. `操作`完运算符栈，最后的`数栈`栈顶即为最终结果

> **操作**：表示用`运算符栈`的栈顶去计算`数栈`的两个数，代码实现如下
```cpp
void eval()
{
    auto b = num.top(); num.pop();
    auto a = num.top(); num.pop();
    auto c = op.top(); op.pop();

    int x;
    if (c == '+') x = a + b;
    else if (c == '-') x = a - b;
    else if (c == '*') x = a * b;
    else x = a / b;
    num.push(x);
}
```

**使用哈希表定义运算符优先级**
数字越大，优先级越高，代码如下
```cpp
unordered_map<char, int> pr{{'+', 1}, {'-', 1}, {'*', 2}, {'/', 2}};
```

**补充：中缀表达式转后缀表达式的实现**
- 将所有和`数栈`有关的代码删掉，改为直接输出符号或者数字即可
	- 因为原题解的思想就是先把`中缀表达式`转化为`后缀表达式`后求解
	- 由于原题解是一个十分简化的模板，所以我们只需要将源代码的转化过程展开即可
> 本代码也可作为模板与表达式求值一同记忆，一同附在了题解中

## 题解
**表达式求值**
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
#include <unordered_map>
#include <stack>

using namespace std;

stack<char> op;
stack<int> num;

void eval()
{
    auto b = num.top(); num.pop();
    auto a = num.top(); num.pop();
    auto c = op.top(); op.pop();

    int x;
    if (c == '+') x = a + b;
    else if (c == '-') x = a - b;
    else if (c == '*') x = a * b;
    else x = a / b;
    num.push(x);
}

int main()
{
    string s;
    cin >> s;

    unordered_map<char, int> pr{{'+', 1}, {'-', 1}, {'*', 2}, {'/', 2}};
    for (int i = 0; i < s.size(); i ++ )
    {
        if (isdigit(s[i]))
        {
            int j = i, x = 0;
            while (j < s.size() && isdigit(s[j]))
                x = x * 10 + s[j ++ ] - '0';
            num.push(x);
            i = j - 1;
        }
        else if (s[i] == '(') op.push(s[i]);
        else if (s[i] == ')')
        {
            while (op.top() != '(') eval();
            op.pop();
        }
        else
        {
            while (op.size() && op.top() != '(' && pr[op.top()] >= pr[s[i]])
                eval();
            op.push(s[i]);
        }
    }

    while (op.size()) eval();
    cout << num.top() << endl;

    return 0;
}
```

**中缀表达式转后缀表达式**
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
#include <unordered_map>
#include <stack>

using namespace std;

stack<char> op;

void eval()
{
    auto c = op.top(); op.pop();
    cout << c << ' ';
}

int main()
{
    string s;
    cin >> s;

    unordered_map<char, int> pr{{'+', 1}, {'-', 1}, {'*', 2}, {'/', 2}};
    for (int i = 0; i < s.size(); i ++ )
    {
        if (isdigit(s[i]))
        {
            int j = i, x = 0;
            while (j < s.size() && isdigit(s[j]))
                x = x * 10 + s[j ++ ] - '0';
            cout << x << ' ';
            i = j - 1;
        }
        else if (s[i] == '(') op.push(s[i]);
        else if (s[i] == ')')
        {
            while (op.top() != '(') eval();
            op.pop();
        }
        else
        {
            while (op.size() && op.top() != '(' && pr[op.top()] >= pr[s[i]])
                eval();
            op.push(s[i]);
        }
    }

    while (op.size()) eval();

    return 0;
}
```
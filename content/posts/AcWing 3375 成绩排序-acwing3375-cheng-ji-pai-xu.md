---
title: AcWing 3375. 成绩排序
date: 2022-07-15 15:18:14.0
updated: 2022-08-02 02:26:28.514
url: /p=12
categories: 
tags: 
- AcWing
- 考研算法
---

## 题目
给定学生的成绩单，成绩单中包含每个学生的姓名和分数，请按照要求将成绩单按成绩从高到低或从低到高的顺序进行重新排列。

对于成绩相同的学生，无论以哪种顺序排列，都要按照原始成绩单中靠前的学生排列在前的规则处理。

**输入格式**
第一行包含整数 $N$，表示学生个数。
第二行包含一个整数 $0$ 或 $1$，表示排序规则，$0$ 表示从高到低，$1$ 表示从低到高。
接下来 $N$ 行，每行描述一个学生的信息，包含一个长度不超过 $10$ 的小写字母构成的字符串表示姓名以及一个范围在 $0∼100$ 的整数表示分数。

**输出格式**
输出重新排序后的成绩单。
每行输出一个学生的姓名和成绩，用单个空格隔开。

**数据范围**
$1≤N≤1000$

**输入样例1：**
```
4
0
jack 70
peter 96
Tom 70
smith 67
```

**输出样例1：**
```
peter 96
jack 70
Tom 70
smith 67
```

**输入样例2：**
```
4
1
jack 70
peter 96
Tom 70
smith 67
```

**输出样例2：**
```
smith 67
jack 70
Tom 70
peter 96
```

## 注意事项及预备知识
注意：
**对于成绩相同的学生，无论以哪种顺序排列，都要按照原始成绩单中靠前的学生排列在前的规则处理。**
这句话，表示我们需要用**稳定排序**。可以用stable_sort

**运算符重载的方法**
```C++
bool operator> (const Person& t) const
{
    return score > t.score;
}
```

## 题解
**stable_sort写法**
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1010;

int n, m;
struct Person
{
    string name;
    int score;

    bool operator< (const Person& t) const
    {
        return score < t.score;
    }
    bool operator> (const Person& t) const
    {
        return score > t.score;
    }
}q[N];

int main()
{
    cin >> n >> m;
    for (int i = 0; i < n; i ++ )
        cin >> q[i].name >> q[i].score;

    if (!m) stable_sort(q, q + n, greater<Person>());
    else stable_sort(q, q + n);

    for (int i = 0; i < n; i ++ )
        cout << q[i].name << ' ' << q[i].score << endl;
    return 0;
}
```

**sort写法**
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1010;

int n, m;
struct Person
{
    string name;
    int score;
    int id;

    bool operator< (const Person& t) const
    {
        if (score != t.score) return score < t.score;
        return id < t.id;
    }
    bool operator> (const Person& t) const
    {
        if (score != t.score) return score > t.score;
        return id < t.id;
    }
}q[N];

int main()
{
    cin >> n >> m;
    for (int i = 0; i < n; i ++ )
    {
        cin >> q[i].name >> q[i].score;
        q[i].id = i;
    }

    if (!m) sort(q, q + n, greater<Person>());
    else sort(q, q + n);

    for (int i = 0; i < n; i ++ )
        cout << q[i].name << ' ' << q[i].score << endl;
    return 0;
}
```
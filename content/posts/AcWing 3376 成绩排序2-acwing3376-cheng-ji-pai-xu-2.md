---
title: AcWing 3376. 成绩排序2
date: 2022-07-15 15:18:39.0
updated: 2022-08-02 02:28:25.817
url: /p=13
categories: 
tags: 
- AcWing
- 考研算法
---

**双关键词排序**

## 题目
给定学生的成绩单，成绩单中包含每个学生的学号和分数，请将成绩单按成绩从低到高的顺序重新排序。
如果学生的成绩相同，则按照学号从小到大的顺序进行排序。

**输入格式**
第一行包含整数 $N$，表示学生数量。
接下来 $N$ 行，每行包含两个整数 $p$ 和 $q$，表示一个学生的学号和成绩。
学生的学号各不相同。

**输出格式**
输出重新排序后的成绩单。
每行输出一个学生的学号和成绩，用单个空格隔开。

**数据范围**
$1≤N≤100$,
$1≤p≤100$,
$0≤q≤100$

**输入样例：**
```
3
1 90
2 87
3 92
```

**输出样例：**
```
2 87
1 90
3 92
```

## 题解
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 110;

int n;
struct Person
{
    int id;
    int score;
    bool operator< (const Person& t) const
    {
        if (score != t.score) return score < t.score;
        return id < t.id;
    }
}q[N];

int main()
{
    cin >> n;
    for (int i = 0; i < n; i ++ ) cin >> q[i].id >> q[i].score;
    sort(q, q + n);

    for (int i = 0; i < n; i ++ ) cout << q[i].id << ' ' << q[i].score << endl;
    return 0;
}
```
---
title: AcWing 840. 模拟散列表
date: 2022-08-10 03:03:15.421
updated: 2022-08-10 08:36:03.418
url: /p=54
categories: 
tags: 
- AcWing
- 考研算法
---

## 题目
维护一个集合，支持如下几种操作：

1. `I x`，插入一个数 $x$；
2. `Q x`，询问数 $x$ 是否在集合中出现过；
现在要进行 $N$ 次操作，对于每个询问操作输出对应的结果。

**输入格式**
第一行包含整数 $N$，表示操作数量。

接下来 $N$ 行，每行包含一个操作指令，操作指令为 `I x`，`Q x` 中的一种。

**输出格式**
对于每个询问指令 `Q x`，输出一个询问结果，如果 $x$ 在集合中出现过，则输出 `Yes`，否则输出 `No`。

每个结果占一行。

**数据范围**
$1≤N≤10^5$
$−10^9≤x≤10^9$

**输入样例：**
```
5
I 1
I 2
I 3
Q 2
Q 5
```

**输出样例：**
```
Yes
No
```

## 思路
**哈希表**
方便查找

**负载因子**
$负载因子=\frac{已有元素数}{数组长度}$

**哈希方法**
一般使用`除余法`，即$h(x)=x \mod n$
以上哈希函数中 $n$ 的选择有很多，比如当 $n$ 为偶数时，$x$ 为偶数则将它存在偶数的位置，$x$ 为奇数则将它存在奇数的位置，$n$ 为奇数时同理。
这样一来，如果 $n$ 中含有小的因子，那么会导致冲突的概率比较大。
所以我们一般将 $n$ 设置为一个较大的质数。

**冲突及其解决**
`冲突`：将要存储的两个数字经过哈希运算后的结果表示要存在同一个位置上。
以 $h(x)=x \mod 3$ 为例，当 $x$ 的值为 $4$ 和 $10$ 时 $h(x)$ 的结果相等，就产生了冲突。
`开散列方法(拉链法)`：在每一个位置开一个单链表，存储元素。
`闭散列方法(开放寻址法)`：当发现当前位置已经有了数字，那么我们就一直寻找到空的位置为止。由于如果是线性的往下搜索空的位置，那么容易产生 `聚集` 问题，比如我们每次加一搜索或者每次加二搜索，那么，`有空空空`就会变为`有有空空`或者`有空有空`，如果碰巧又有一个数字，则排列就会变为`有有有空`，另一种也是`有有有空`。所以我们可以使用非线性的，比如`d`，`d+1^2`，`d-1^2`，`d+2^2`，`d-2^2`这样就尽可能地避免了问题。
自己实现探查一般使用线性的即可。

## 题解 - yxc版本
**开散列方法（拉链法）**
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 200003;

int n;
int h[N], e[N], ne[N], idx;

bool find(int x)
{
    int t = (x % N + N) % N;
    for (int i = h[t]; ~i; i = ne[i])
        if (e[i] == x)
            return true;
    return false;
}

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

void insert(int x)
{
    if (find(x)) return;
    int t = (x % N + N) % N;
    add(t, x);
}

int main()
{
    memset(h, -1, sizeof h);
    scanf("%d", &n);
    while (n -- )
    {
        char op[2];
        int x;
        scanf("%s%d", op, &x);
        if (*op == 'I') insert(x);
        else
        {
            if (find(x)) puts("Yes");
            else puts("No");
        }
    }

    return 0;
}
```
**闭散列方法（开放寻址法）**
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 200003, null = 0x3f3f3f3f;

int n;
int h[N];

int find(int x)
{
    int t = (x % N + N) % N;
    while (h[t] != null && h[t] != x)
        t = (t + 1) % N;
    return t;
}

int main()
{
    memset(h, 0x3f, sizeof h);
    scanf("%d", &n);
    while (n -- )
    {
        char op[2];
        int x;
        scanf("%s%d", op, &x);
        if (*op == 'I') h[find(x)] = x;
        else
        {
            if (h[find(x)] == null) puts("No");
            else puts("Yes");
        }
    }

    return 0;
}
```

## 其他版本代码
**拉链法代码**
```cpp
/*
 * Project: 11_哈希表
 * File Created:Sunday, January 17th 2021, 2:11:23 pm
 * Author: Bug-Free
 * Problem:AcWing 840. 模拟散列表 拉链法
 */
#include <cstring>
#include <iostream>

using namespace std;

const int N = 1e5 + 3;  // 取大于1e5的第一个质数，取质数冲突的概率最小 可以百度

//* 开一个槽 h
int h[N], e[N], ne[N], idx;  //邻接表

void insert(int x) {
    // c++中如果是负数 那他取模也是负的 所以 加N 再 %N 就一定是一个正数
    int k = (x % N + N) % N;
    e[idx] = x;
    ne[idx] = h[k];
    h[k] = idx++;
}

bool find(int x) {
    //用上面同样的 Hash函数 讲x映射到 从 0-1e5 之间的数
    int k = (x % N + N) % N;
    for (int i = h[k]; i != -1; i = ne[i]) {
        if (e[i] == x) {
            return true;
        }
    }
    return false;
}

int n;

int main() {
    cin >> n;

    memset(h, -1, sizeof h);  //将槽先清空 空指针一般用 -1 来表示

    while (n--) {
        string op;
        int x;
        cin >> op >> x;
        if (op == "I") {
            insert(x);
        } else {
            if (find(x)) {
                puts("Yes");
            } else {
                puts("No");
            }
        }
    }
    return 0;
}
```
**开放寻址法代码**
```cpp
/*
 * Project: 11_哈希表
 * File Created:Sunday, January 17th 2021, 4:39:01 pm
 * Author: Bug-Free
 * Problem:AcWing 840. 模拟散列表  开放寻址法
 */
#include <cstring>
#include <iostream>

using namespace std;

//开放寻址法一般开 数据范围的 2~3倍, 这样大概率就没有冲突了
const int N = 2e5 + 3;        //大于数据范围的第一个质数
const int null = 0x3f3f3f3f;  //规定空指针为 null 0x3f3f3f3f

int h[N];

int find(int x) {
    int t = (x % N + N) % N;
    while (h[t] != null && h[t] != x) {
        t++;
        if (t == N) {
            t = 0;
        }
    }
    return t;  //如果这个位置是空的, 则返回的是他应该存储的位置
}

int n;

int main() {
    cin >> n;

    memset(h, 0x3f, sizeof h);  //规定空指针为 0x3f3f3f3f

    while (n--) {
        string op;
        int x;
        cin >> op >> x;
        if (op == "I") {
            h[find(x)] = x;
        } else {
            if (h[find(x)] == null) {
                puts("No");
            } else {
                puts("Yes");
            }
        }
    }
    return 0;
}
```
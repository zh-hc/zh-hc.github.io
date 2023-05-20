---
title: AcWing 52. 数组中出现次数超过一半的数字
date: 2022-08-25 08:18:03.843
updated: 2022-08-25 08:39:36.96
url: /p=60
categories: 
tags: 
- AcWing
- 考研算法
---

## 题目
数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。

假设数组非空，并且一定存在满足条件的数字。

**思考题：**

- 假设要求只能使用 $O(n)$ 的时间和额外 $O(1)$ 的空间，该怎么做呢？

**数据范围**
数组长度 $[1,1000]$。

**样例**
```
输入：[1,2,1,1,3]

输出：1
```

## 思路
如果一个数大于一半，这个数最后的数量肯定大于其他所有数的数量之和。

## 题解
```cpp
class Solution {
public:
    int moreThanHalfNum_Solution(vector<int>& nums) {
        int cnt = 0, val;
        for (auto x: nums) {
            if (!cnt) val = x, cnt ++ ;
            else if (x == val) cnt ++ ;
            else cnt -- ;
        }

        return val;
    }
};
```
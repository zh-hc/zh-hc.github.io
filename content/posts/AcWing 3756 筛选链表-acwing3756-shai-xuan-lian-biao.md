---
title: AcWing 3756. 筛选链表
date: 2022-07-20 15:21:42.63
updated: 2022-08-03 05:08:54.262
url: /p=18
categories: 
tags: 
- AcWing
- 考研算法
---

## 题目
一个单链表中有 $m$ 个结点，每个结点上的元素的绝对值不超过 $n$。
现在，对于链表中元素的绝对值相等的结点，仅保留第一次出现的结点而删除其余绝对值相等的结点。
请输出筛选后的新链表。
例如，单链表`21 -> -15 -> -15 -> -7 -> 15`，在进行筛选和删除后，变为`21 -> -15 -> -7`。

**输入样例：**
```
输入：21->-15->-15->-7->15

输出：21->-15->-7
```

**数据范围**
$1≤m≤1000$,
$1≤n≤10000$

## 思路
**如何判断一个数是否出现过**
可以定义一个长度为 $n$ 的布尔数组 $st$，来存储这个下标对应的数字(注意要取绝对值)是否出现过

**如何进行链表的删除操作**
按照常规思路，当我们发现某一个数值需要删除时，我们需要`从头开始`重新遍历到他的上一个结点。
转换思路，我们可以`直接在上一个结点`判断是否要删除该结点。
```cpp
int x = abs(p->next->val);
if (st[x]) {
    auto q = p->next;
    p->next = q->next;
    delete q;
} else {
    p = p->next;
    st[x] = true;
}
```

## 题解
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* filterList(ListNode* head) {
        bool st[10001] = {};
        st[abs(head->val)] = true;
        for (auto p = head; p->next;) {
            int x = abs(p->next->val);
            if (st[x]) {
                auto q = p->next;
                p->next = q->next;
                delete q;
            } else {
                p = p->next;
                st[x] = true;
            }
        }
        return head;
    }
};
```
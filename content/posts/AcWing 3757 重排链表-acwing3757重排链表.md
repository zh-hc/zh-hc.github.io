---
title: AcWing 3757. 重排链表
date: 2022-07-21 02:18:42.112
updated: 2022-08-03 05:13:04.513
url: /p=19
categories: 
tags: 
- AcWing
- 考研算法
---

## 题目
一个包含 n 个元素的线性链表 $L=(a_{1},a_{2},…,a_{n−2},a_{n−1},a_n)$。
现在要对其中的结点进行重新排序，得到一个新链表 $L^′=(a_1,a_n,a_2,a_{n−1},a_3,a_{n−2}…)$

**样例1：**
```
输入：1->2->3->4

输出：1->4->2->3
```

**样例2：**
```
输入：1->2->3->4->5

输出：1->5->2->4->3
```

**数据范围**
$1≤n≤1000$,
$1≤a_i≤10000$

## 思路
**注意**
原题中限制了空间复杂度为 $O(1)$，也就是不让开新的数组

**用到的方法**
是`反转链表`和`合并链表`的结合

**大概思路**
- 首先将整个链表从中间截开
- 将后半部分链表反转
- 将两个链表合并即可

**具体思路**
- 首先确定第一部分链表的尾部和第二部分链表的头部
	- 求链表的长度(枚举每个点，将结果存储在n中)
	- 对前半段进行处理
		- 求得前半段的节点数： $\lceil \frac {n} {2} \rceil$，即`(n+1)/2`
		- 将前半段链表赋值给a
    - 对后半段进行处理(b和c表示相邻的两个节点，反转时遍历两个指针，将`b指向c`改为`c指向b`即可)
    	- b = a->next 得到后半段的起点b
    	- c = b->next 定义c是b的相邻节点
    - 断开两部分
    	- a->next = b->next = NULL;
- 翻转第二部分链表
  	```cpp
    while (c) {
        auto p = c->next;
        c->next = b;
        b = c, c = p;
    }
  	```
	> 此时，两部分链表的头结点分别为a、b
- 合并两个链表
  ```cpp
  // p表示前半段 q表示后半段
  for (auto p = head, q = b; q;) {
      auto o = q->next;
      q->next = p->next;
      p->next = q;
      p = p->next->next;
      q = o;
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
    void rearrangedList(ListNode* head) {
        if (!head->next) return;
        int n = 0;
        for (auto p = head; p; p = p->next) n ++ ;
        int left = (n + 1) / 2;  // 前半段的节点数
        auto a = head;
        for (int i = 0; i < left - 1; i ++ ) a = a->next;
        auto b = a->next, c = b->next;

        a->next = b->next = NULL;
        while (c) {
            auto p = c->next;
            c->next = b;
            b = c, c = p;
        }

        for (auto p = head, q = b; q;) {
            auto o = q->next;
            q->next = p->next;
            p->next = q;
            p = p->next->next;
            q = o;
        }
    }
};
```
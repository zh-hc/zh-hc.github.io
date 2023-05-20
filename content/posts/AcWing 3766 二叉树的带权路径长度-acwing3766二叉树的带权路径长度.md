---
title: AcWing 3766. 二叉树的带权路径长度
date: 2022-07-29 06:49:37.248
updated: 2022-08-03 05:30:17.428
url: /p=42
categories: 
tags: 
- AcWing
- 考研算法
---

## 题目
二叉树的带权路径长度(WPL)是二叉树中所有叶结点的带权路径长度之和，也就是每个叶结点的深度与权值之积的总和。
给定一棵二叉树 $T$，请你计算并输出它的 WPL。
注意，根节点的深度为 $0$。

**样例**
```
输入：二叉树[8, 12, 2, null, null, 6, 4, null, null, null, null]如下图所示：
    8
   / \
  12  2
     / \
    6   4

输出：32
```

**数据范围**
二叉树结点数量不超过 $1000$。
每个结点的权值均为不超过 $100$ 的非负整数。

## 思路
这是一个经典的`树的遍历`问题
- 一般有两种实现方法
	- `DFS`(深度优先搜索)(也就是`递归`)
	- `BFS`(广度优先搜索)

> 建议使用`DFS`实现，因为它不需要自己实现队列，代码会比BFS实现短很多

- DFS步骤：
	1. 处理根节点(root)
	2. 递归左子树`dfs(root->left)`
	3. 递归右子树`dfs(root->right)`

## 题解
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int dfs(TreeNode* root, int depth) {
        if (!root) return 0;
        if (!root->left && !root->right) return root->val * depth;
        return dfs(root->left, depth + 1) + dfs(root->right, depth + 1);
    }

    int pathSum(TreeNode* root) {
        return dfs(root, 0);
    }
};
```
---
title: AcWing 18. 重建二叉树
date: 2022-07-31 03:48:13.712
updated: 2022-08-03 05:33:27.186
url: /p=44
categories: 
tags: 
- AcWing
- 考研算法
---

## 题目
输入一棵二叉树前序遍历和中序遍历的结果，请重建该二叉树。

**注意:**

- 二叉树中每个节点的值都互不相同；
- 输入的前序遍历和中序遍历一定合法；

**数据范围**
树中节点数量范围 $[0,100]$。

**样例**
```
给定：
前序遍历是：[3, 9, 20, 15, 7]
中序遍历是：[9, 3, 15, 20, 7]

返回：[3, 9, 20, null, null, 15, 7, null, null, null, null]
返回的二叉树如下所示：
    3
   / \
  9  20
    /  \
   15   7   
```

## 思路
**算法**
**(递归)** $O(n)$
递归建立整棵二叉树：先递归创建左右子树，然后创建根节点，并让指针指向两棵子树。

前序遍历性质： 节点按照`[ 根节点 | 左子树 | 右子树 ]`排序。
中序遍历性质： 节点按照`[ 左子树 | 根节点 | 右子树 ]`排序。

> 以题目示例为例：
前序遍历划分`[ 3 | 9 | 20 15 7 ]`
中序遍历划分`[ 9 | 3 | 15 20 7 ]`

![image](upload/2022/08/image.png)

具体步骤如下：

1. 先利用`前序遍历`找`根节点`：前序遍历的`第一个数`，就是根节点的值；
2. 在`中序遍历`中找到根节点的位置 $k$，则 $k$ 左边是左子树的中序遍历，右边是右子树的中序遍历；
3. 假设左子树的中序遍历的长度是 $l$，则在前序遍历中，根节点后面的 $l$ 个数，是左子树的前序遍历，剩下的数是右子树的前序遍历；
4. 有了左右子树的前序遍历和中序遍历，我们可以先递归创建出左右子树，然后再创建根节点；

**时间复杂度分析**
我们在初始化时，用`哈希表`(`unordered_map<int,int>`)记录每个值在中序遍历中的位置，这样我们在递归到每个节点时，在中序遍历中查找根节点位置的操作，只需要 $O(1)$ 的时间。此时，创建每个节点需要的时间是 $O(1)$ ，所以总时间复杂度是 $O(n)$。

> **注意**：如果是`笔试`的话，因为`不可以`使用STL的哈希函数，我们可以将`哈希表`改为`线性查找`，创建每个节点需要的时间从 $O(1)$ 变为 $O(n)$。

```cpp
// 原哈希表写法 上机题用这个即可
int k = pos[root->val];

// 笔试写法 写循环即可
// 优化方法：开放寻址法、拉链法，在这里没有实现
int k = -1;
for (int i = x; i <= y; i++ )
    if (inorder[i] == root->val)
        k = i;
```

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
    unordered_map<int, int> pos;
    vector<int> preorder, inorder;

    TreeNode* build(int a, int b, int x, int y) {
        if (a > b) return NULL;
        auto root = new TreeNode(preorder[a]);
        int k = pos[root->val];
        root->left = build(a + 1, a + 1 + k - 1 - x, x, k - 1);
        root->right = build(a + 1 + k - 1 - x + 1, b, k + 1, y);
        return root;
    }

    TreeNode* buildTree(vector<int>& _preorder, vector<int>& _inorder) {
        preorder = _preorder, inorder = _inorder;
        int n = inorder.size();
        for (int i = 0; i < n; i ++ ) pos[inorder[i]] = i;
        return build(0, n - 1, 0, n - 1);
    }
};
```

## 参考链接
https://leetcode.cn/leetbook/read/illustration-of-algorithm/99ljye/

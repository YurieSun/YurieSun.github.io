---
layout: post
title: LeetCode 0653 题解
description: "两数之和 IV - 输入 BST"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[两数之和 IV - 输入 BST](https://leetcode-cn.com/problems/two-sum-iv-input-is-a-bst/)

### 题解
1. 法一（中序遍历）：对BST进行中序遍历得到的是有序序列，则在遍历过程中更新相邻两节点差的最小值即可。
```java
class Solution {
    TreeNode pre = null;
    int res = Integer.MAX_VALUE;
    public int getMinimumDifference(TreeNode root) {
        inOrder(root);
        return res;
    }
    private void inOrder(TreeNode root){
        if(root == null)
            return;
        inOrder(root.left);
        if(pre != null)
            res = Math.min(res, root.val - pre.val);
        pre = root;
        inOrder(root.right);
    }
}
```
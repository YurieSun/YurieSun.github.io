---
layout: post
title: LeetCode 0543 题解
description: "二叉树的直径"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

### 题解
1. 法一（递归）
```java
class Solution {
    private int max;
    public int diameterOfBinaryTree(TreeNode root) {
        maxDepth(root);
        return max;
    }
    public int maxDepth(TreeNode root){
        if(root == null)
            return 0;
        int l = maxDepth(root.left);
        int r = maxDepth(root.right);
        max= Math.max(max, l + r);
        return Math.max(l, r) + 1;
    }
}
```
* 注意：直径不一定要经过根节点，因此在计算树的高度时应该同时更新`max`。
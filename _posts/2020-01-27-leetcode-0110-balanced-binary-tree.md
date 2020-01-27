---
layout: post
title: LeetCode 0110 题解
description: "平衡二叉树"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)

### 题解
1. 法一（递归）
```java
class Solution {
    private boolean result = true;

    public boolean isBalanced(TreeNode root) {
        maxDepth(root);
        return result;
    }
    private int maxDepth(TreeNode root){
        if(root == null)
            return 0;
        int l = maxDepth(root.left);
        int r = maxDepth(root.right);
        if(Math.abs(l - r) > 1)
            result = false;
        return Math.max(l, r) + 1;
    }
}
```
* 关于解决递归问题的方法可参考 [三道题套路解决递归问题](https://lyl0724.github.io/2020/01/25/1/)。
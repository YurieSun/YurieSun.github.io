---
layout: post
title: LeetCode 0235 题解
description: "二叉搜索树的最近公共祖先"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

### 题解
1. 法一（递归）：对于最近公共祖先`root`，`p`和`q`必须满足分别在`root`的左子树和右子树上，或者`p`、`q`作为祖先。
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root.val > p.val && root.val > q.val)
            return lowestCommonAncestor(root.left, p, q);
        if(root.val < p.val && root.val < q.val)
            return lowestCommonAncestor(root.right, p, q);
        return root;
    }
}
```
2. 法二（迭代）：思路与法一一样。
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        TreeNode node = root;
        while(node != null){
            if(node.val > p.val && node.val > q.val)
                node = node.left;
            else if(node.val < p.val && node.val < q.val)
                node = node.right;
            else
                return node;
        }
        return null;
    }
}
```
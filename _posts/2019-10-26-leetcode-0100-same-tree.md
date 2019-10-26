---
layout: post
title: LeetCode 0100 题解
description: "相同的树"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[相同的树](https://leetcode-cn.com/problems/same-tree/)

### 思路
先查看根节点，随后递归查看左子树和右子树直到遇到空节点；在查看过程中一旦遇到不相等的元素即返回。

### 题解
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if(p == null & q == null) return true;
		if(p == null || q == null) return false;
		if(p.val != q.val) return false;
		return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
    }
}
```
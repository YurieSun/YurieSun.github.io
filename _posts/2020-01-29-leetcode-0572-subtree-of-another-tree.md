---
layout: post
title: LeetCode 0572 题解
description: "另一个树的子树"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[另一个树的子树](https://leetcode-cn.com/problems/subtree-of-another-tree/)

### 题解
1. 法一（双重递归）
```java
class Solution {
    public boolean isSubtree(TreeNode s, TreeNode t) {
        if(s == null)
            return false;
        // 若t是s的子树，有三种情况：s和t相等，t是s左子树的子树，t是s右子树的子树。
        return isEqual(s, t) || isSubtree(s.left, t) || isSubtree(s.right ,t);
    }
    private boolean isEqual(TreeNode s, TreeNode t){
        if(s == null && t == null)
            return true;
        if(s == null || t == null)
            return false;
        if(s.val != t.val)
            return false;
        return isEqual(s.left, t.left) && isEqual(s.right, t.right);
    }
}
```
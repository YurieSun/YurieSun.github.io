---
layout: post
title: 剑指Offer 26 题解
description: "树的子结构"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)

### 题解
1. 法一（双重递归）：这一题和[Leetcode 0572 另一个树的子树](https://leetcode-cn.com/problems/subtree-of-another-tree/)做法相似，只不过对于子树和子结构的定义不一样，子树需要每一个节点都有，不能缺少，而子结构可以缺少某些节点，相当于子结构是子树的一部分。因此，判定子树的`isEqual`函数中，若`s`非空而`t`空，需返回`false`；判定子结构的`isEqual`函数中，若`s`非空而`t`空，需返回`true`。且对于空树的处理也不一样，即空树不是任意树的子结构，但空树是除空树外任意树的子树；因此主函数的递归停止条件也不同。
```java
class Solution {
    public boolean isSubStructure(TreeNode A, TreeNode B) {
        if(A == null || B == null)
            return false;
        return isEqual(A,B) || isSubStructure(A.left,B) || isSubStructure(A.right,B);
    }
    private boolean isEqual(TreeNode s, TreeNode t){
        if(t == null)
            return true;
        if(s == null)
            return false;
        return s.val == t.val && isEqual(s.left,t.left) && isEqual(s.right, t.right);
    }
}
```
* 附LeetCode 0572的代码
```java
class Solution {
    public boolean isSubtree(TreeNode s, TreeNode t) {
        if(s == null)
            return false;
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
---
layout: post
title: LeetCode 0671 题解
description: "二叉树中第二小的节点"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[二叉树中第二小的节点](https://leetcode-cn.com/problems/second-minimum-node-in-a-binary-tree/)

### 题解
1. 法一（递归）：根据题目中树的结构，从根节点出发只要找到与根节点不一样的值，就是第二小的值。
```java
class Solution {
    public int findSecondMinimumValue(TreeNode root) {
        if(root == null)
            return -1;
        if(root.left == null && root.right == null)
            return -1;
        int leftVal = root.left.val;
        int rightVal = root.right.val;
        if(root.val == leftVal)
            leftVal = findSecondMinimumValue(root.left);
        if(root.val == rightVal)
            rightVal = findSecondMinimumValue(root.right);
        if(leftVal != -1 && rightVal != -1)
            return Math.min(leftVal, rightVal);
        else   
            return Math.max(leftVal, rightVal); 
    }
}
```
2. 法二（递归）：根据题目，可转换成找左右节点的最小值。
```java
class Solution {
    public int findSecondMinimumValue(TreeNode root) {
        return findMin(root, root.val);
    }
    private int findMin(TreeNode root, int min){
        if(root == null)
            return -1;
        if(root.val > min)
            return root.val;
        int leftVal = findMin(root.left, min);
        int rightVal = findMin(root.right, min);
        if(leftVal != -1 && rightVal != -1)
            return Math.min(leftVal, rightVal);
        return Math.max(leftVal, rightVal);
    }
}
```
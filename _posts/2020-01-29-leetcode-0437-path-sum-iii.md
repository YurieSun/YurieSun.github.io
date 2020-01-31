---
layout: post
title: LeetCode 0437 题解
description: "路径总和 III"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[路径总和 III](https://leetcode-cn.com/problems/path-sum-iii/)

### 题解
1. 法一（双重递归）
```java
class Solution {
    public int pathSum(TreeNode root, int sum) {
        if(root == null)
            return 0;
        int count = pathSumStartWithRoot(root, sum) + pathSum(root.left, sum) + pathSum(root.right, sum);
        return count;
    }
    private int pathSumStartWithRoot(TreeNode root, int sum){
        if(root == null)
            return 0;
        int count = 0;
        if(root.val == sum)
            count++;
        count += pathSumStartWithRoot(root.left, sum - root.val) + pathSumStartWithRoot(root.right, sum - root.val);
        return count;
    }
}
```
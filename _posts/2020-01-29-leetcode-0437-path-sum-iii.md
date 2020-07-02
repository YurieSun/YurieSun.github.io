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
2. 法二（回溯）
```java
class Solution {
    public int pathSum(TreeNode root, int sum) {
        if (root == null)
            return 0;
        HashMap<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);
        return helper(root, sum, map, 0);
    }
    public int helper(TreeNode root, int sum, HashMap<Integer, Integer> map, int pathSum) {
        // 终止条件
        if (root == null)
            return 0;
        // 本层要做的事
        int res = 0;
        pathSum += root.val;
        res += map.getOrDefault(pathSum - sum, 0);
        map.put(pathSum, map.getOrDefault(pathSum, 0) + 1);
        // 递归计算下一层
        res += helper(root.left, sum, map, pathSum) + helper(root.right, sum, map, pathSum);
        // 回溯
        map.put(pathSum, map.get(pathSum) - 1);
        return res;
    }
}
```
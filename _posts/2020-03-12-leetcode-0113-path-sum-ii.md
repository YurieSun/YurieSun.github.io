---
layout: post
title: LeetCode 0113 题解
description: "路径总和 II"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[路径总和 II](https://leetcode-cn.com/problems/path-sum-ii/)

### 题解
1. 法一（回溯）：相当于在遍历所有路径，寻找哪一条路径是满足条件的。
```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        backtracking(root, sum, new ArrayList<Integer>());
        return res;
    }
    private void backtracking(TreeNode node, int sum, ArrayList<Integer> path) {
        if (node == null)
            return;
        path.add(node.val);
        sum -= node.val;
        if (sum == 0 && node.left == null && node.right == null)
            res.add(new ArrayList<Integer>(path));
        else {
            backtracking(node.left, sum, path);
            backtracking(node.right, sum, path);
        }
        path.remove(path.size() - 1);
    }
}
```
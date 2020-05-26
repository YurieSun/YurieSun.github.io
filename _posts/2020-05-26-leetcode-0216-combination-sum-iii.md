---
layout: post
title: LeetCode 0040 题解
description: "组合总和 III"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[组合总和 III](https://leetcode-cn.com/problems/combination-sum-iii/)

### 题解
1. 法一（回溯）
```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> combinationSum3(int k, int n) {
        if (k <= 0 || n <= 0 || k > 9)
            return res;
        backtrace(k, n, 0, 1, new ArrayList<Integer>());
        return res;
    }
    private void backtrace(int k, int n, int sum, int start, ArrayList<Integer> path) {
        if (path.size() == k) {
            if (sum == n)
                res.add(new ArrayList<>(path));
            return;
        }
        for (int i = start; i <= 9; i++) {
            path.add(i);
            backtrace(k, n, sum + i, i + 1, path);
            path.remove(path.size() - 1);
        }
    }
}
```
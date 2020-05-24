---
layout: post
title: LeetCode 0039 题解
description: "组合总和"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[组合总和](https://leetcode-cn.com/problems/combination-sum/)

### 题解
1. 法一（回溯）：本题中[2,2,3]和[2,3,2]是同一个解，因此属于组合问题，在递归函数中i需要从记录的start开始。
```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        if (candidates == null || candidates.length == 0)
            return res;
        backtrace(candidates, target, 0, new ArrayList<Integer>());
        return res;
    }
    private void backtrace(int[] nums, int sum, int start, ArrayList<Integer> path) {
        if (sum < 0) {
            return;
        }
        if (sum == 0) {
            res.add(new ArrayList<>(path));
            return;
        }
        for (int i = start; i < nums.length; i++) {
            path.add(nums[i]);
            // 由于元素可重复使用，因此这里传入的start为i。
            backtrace(nums, sum - nums[i], i, path);
            path.remove(path.size() - 1);
        }
    }
}
```
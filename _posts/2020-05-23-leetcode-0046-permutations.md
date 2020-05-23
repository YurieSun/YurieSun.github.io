---
layout: post
title: LeetCode 0046 题解
description: "全排列"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[全排列](https://leetcode-cn.com/problems/permutations/)

### 题解
1. 法一（回溯）
```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    boolean[] visited;
    public List<List<Integer>> permute(int[] nums) {
        if (nums == null || nums.length == 0)
            return res;
        visited = new boolean[nums.length];
        backtrace(nums, new ArrayList<Integer>());
        return res;

    }
    private void backtrace(int[] nums, ArrayList<Integer> path) {
        if (path.size() == nums.length) {
            res.add(new ArrayList(path));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (visited[i])
                continue;
            path.add(nums[i]);
            visited[i] = true;
            backtrace(nums, path);
            path.remove(path.size() - 1);
            visited[i] = false;
        }
    }
}
```
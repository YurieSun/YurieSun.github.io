---
layout: post
title: LeetCode 0078 题解
description: "子集"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[子集](https://leetcode-cn.com/problems/subsets/)

### 题解
1. 法一（回溯）
```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> subsets(int[] nums) {
        if (nums == null || nums.length == 0)
            return res;
        backtrace(nums, 0, new ArrayList<>());
        return res;
    }
    private void backtrace(int[] nums, int start, ArrayList<Integer> path) {
        // 不需要走到树的叶子节点才返回，每一个节点都需要记录下来。
        res.add(new ArrayList<>(path));
        for (int i = start; i < nums.length; i++) {
            path.add(nums[i]);
            backtrace(nums, i + 1, path);
            path.remove(path.size() - 1);
        }
    }
}
```
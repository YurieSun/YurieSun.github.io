---
layout: post
title: LeetCode 0090 题解
description: "子集 II"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[子集 II](https://leetcode-cn.com/problems/subsets-ii/)

### 题解
1. 法一（回溯）
```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        if (nums == null || nums.length == 0)
            return res;
        // 提前排序
        Arrays.sort(nums);
        backtrace(nums, 0, new ArrayList<Integer>());
        return res;
    }
    private void backtrace(int[] nums, int start, ArrayList<Integer> path) {
        res.add(new ArrayList<>(path));
        for (int i = start; i < nums.length; i++) {
            // 对于重复元素的剪枝，也可使用visited数组实现。
            if (i > start && nums[i - 1] == nums[i])
                continue;
            path.add(nums[i]);
            backtrace(nums, i + 1, path);
            path.remove(path.size() - 1);
        }
    }
}
```
---
layout: post
title: LeetCode 0040 题解
description: "组合总和 II"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/)

### 题解
1. 法一（回溯）
```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    boolean[] visited;
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        if (candidates == null || candidates.length == 0)
            return res;
        visited = new boolean[candidates.length];
        // 提前排序，为剪枝做准备。
        Arrays.sort(candidates);
        backtrace(candidates, 0, target, new ArrayList<Integer>());
        return res;
    }
    private void backtrace(int[] nums, int start, int sum, ArrayList<Integer> path) {
        if (sum < 0) {
            return;
        }
        if (sum == 0) {
            res.add(new ArrayList<>(path));
            return;
        }
        for (int i = start; i < nums.length; i++) {
            if (visited[i])
                continue;
            // 去掉重复元素的解
            if (i != 0 && nums[i - 1] == nums[i] && !visited[i - 1])
                continue;
            visited[i] = true;
            path.add(nums[i]);
            // 由于每个元素只能用一次，因此这里传入的start为i+1。
            backtrace(nums, i + 1, sum - nums[i], path);
            visited[i] = false;
            path.remove(path.size() - 1);
        }
    }
}
```
* 改进：可以去掉`visited`数组，同样达到剪枝的效果。
```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        if (candidates == null || candidates.length == 0)
            return res;
        Arrays.sort(candidates);
        backtrace(candidates, 0, target, new ArrayList<Integer>());
        return res;
    }
    private void backtrace(int[] nums, int start, int sum, ArrayList<Integer> path) {
        if (sum < 0) {
            return;
        }
        if (sum == 0) {
            res.add(new ArrayList<>(path));
            return;
        }
        for (int i = start; i < nums.length; i++) {
            // 对于相同的元素，在树的同一层中只能出现一个，而在不同层中则可以出现多次；
            // 因此i>start表明在同一层中不是第一次出现该元素的情况，就需要剪掉。
            if (i > start && nums[i - 1] == nums[i])
                continue;
            path.add(nums[i]);
            backtrace(nums, i + 1, sum - nums[i], path);
            path.remove(path.size() - 1);
        }
    }
}
```
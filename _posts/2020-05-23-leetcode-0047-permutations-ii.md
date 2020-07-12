---
layout: post
title: LeetCode 0047 题解
description: "全排列 II"
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
    public List<List<Integer>> permuteUnique(int[] nums) {
        if (nums == null || nums.length == 0)
            return res;
        visited = new boolean[nums.length];
        // 对数组进行预处理：排序
        Arrays.sort(nums);
        backtrace(nums, new ArrayList<Integer>());
        return res;
    }
    private void backtrace(int[] nums, ArrayList<Integer> path) {
        if (path.size() == nums.length) {
            res.add(new ArrayList<>(path));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (visited[i])
                continue;
            // 对于重复元素的剪枝：如果遇到相同元素且上一个元素是被回溯过的（也就是上一个元素被标记为未访问），说明该元素的结果已经被记录，可以跳过。
            if (i != 0 && nums[i - 1] == nums[i] && !visited[i - 1])
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
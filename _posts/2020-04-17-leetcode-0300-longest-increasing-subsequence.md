---
layout: post
title: LeetCode 0300 题解
description: "最长上升子序列"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

### 题解
1. 法一（动态规划）：`dp[i]`的含义为，以`nums[i]`作为子序列结尾数字时的最长上升子序列长度。因此，对于每个数`nums[i]`，遍历它前面的所有数`nums[j]`，如果`nums[i]`更大，说明可以加到`nums[j]`后面形成一个更长的上升子序列，由此可以得到状态转移方程 $dp[i]=\max \limits_{0\le j<i} (dp[j]+1), if(nums[i]>nums[j]) $。
```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        if (nums == null || nums.length == 0)
            return 0;
        int[] dp = new int[nums.length];
        Arrays.fill(dp, 1);
        for (int i = 0; i < nums.length; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
        }
        int res = 0;
        for (int x : dp) {
            res = Math.max(res, x);
        }
        return res;
    }
}
```
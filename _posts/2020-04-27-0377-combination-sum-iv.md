---
layout: post
title: LeetCode 0377 题解
description: "组合总和 Ⅳ"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[组合总和 Ⅳ](https://leetcode-cn.com/problems/combination-sum-iv/)

### 题解
1. 法一（动态规划）：和 [零钱兑换 II](https://leetcode-cn.com/problems/coin-change-2/) 不一样的地方在于，零钱兑换中顺序不同算同一种方案，而本题中顺序不同是不同的方案，即有顺序的背包问题。这时需要将物品的循环放在最内层。
```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        int[] dp = new int[target + 1];
        dp[0] = 1;
        for (int i = 1; i <= target; i++) {
            for (int j = 0; j < nums.length; j++) {
                if (i >= nums[j]) {
                    dp[i] += dp[i - nums[j]];
                }
            }
        }
        return dp[target];
    }
}
```
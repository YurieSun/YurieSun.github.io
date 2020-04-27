---
layout: post
title: LeetCode 0322 题解
description: "零钱兑换 II"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[零钱兑换 II](https://leetcode-cn.com/problems/coin-change-2/)

### 题解
1. 法一（动态规划）：和 [零钱兑换](https://leetcode-cn.com/problems/coin-change/) 一样，只不过变成了求方案数，此时只需将其中的`min`改为`sum`，同时将初始条设为`dp[0]=1`即可。
```java
class Solution {
    public int change(int amount, int[] coins) {
        int[] dp = new int[amount + 1];
        dp[0] = 1;
        for (int i = 0; i < coins.length; i++) {
            for (int j = coins[i]; j <= amount; j++) {
                dp[j] += dp[j - coins[i]];
            }
        }
        return dp[amount];
    }
}
```
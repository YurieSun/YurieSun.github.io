---
layout: post
title: LeetCode 0322 题解
description: "零钱兑换"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[零钱兑换](https://leetcode-cn.com/problems/coin-change/)

### 题解
1. 法一（动态规划）：这是一个完全背包问题，其中背包容量为`amount`，每个物品的体积和价值分别为硬币的面值`coins[i]`和`1`。因此就是求在上述条件下，将背包恰好装满的最小价值。由于是恰好装满的问题，因此初始化只有`dp[0]=0`，其余位置设为 $+\infty$ 。如果用单个硬币组成`amount`，最多使用`amount`个（用面额为1的硬币恰好为`amount`），因此将其余位置设为`amount+1`，相当于是 $+\infty$ 。
```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1);
        dp[0] = 0;
        for (int i = 0; i < coins.length; i++) {
            for (int j = coins[i]; j <= amount; j++) {
                dp[j] = Math.min(dp[j], dp[j - coins[i]] + 1);
            }
        }
        return dp[amount] == amount + 1 ? -1 : dp[amount];
    }
}
```
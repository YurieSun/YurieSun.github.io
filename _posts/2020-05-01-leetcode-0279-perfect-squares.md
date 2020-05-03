---
layout: post
title: LeetCode 0279 题解
description: "完全平方数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[完全平方数](https://leetcode-cn.com/problems/perfect-squares/)

### 题解
1. 法一（动态规划）
```java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            // 先将每个值设为最大
            dp[i] = i;
            for (int j = 1; j * j <= i; j++) {
                dp[i] = Math.min(dp[i], dp[i - j * j] + 1);
            }
        }
        return dp[n];
    }
}
```
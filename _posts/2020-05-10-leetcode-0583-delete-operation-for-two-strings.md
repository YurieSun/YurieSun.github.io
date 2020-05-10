---
layout: post
title: LeetCode 0583 题解
description: "两个字符串的删除操作"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[两个字符串的删除操作](https://leetcode-cn.com/problems/delete-operation-for-two-strings/)

### 题解
1. 法一（动态规划）：转换成求两个字符串的最长公共子序列问题，改变最终的返回值即可。
```java
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length(), n = word2.length();
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return m + n - 2 * dp[m][n];
    }
}
```
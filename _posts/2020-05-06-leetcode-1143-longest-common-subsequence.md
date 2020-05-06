---
layout: post
title: LeetCode 1143 题解
description: "最长公共子序列"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)

### 题解
1. 法一（动态规划）：定义数组`dp[i][j]`表示`text1`的前i个字符和`text2`的前j个字符的最长公共子序列长度。若`text1[i]==text2[j]`，则`dp[i][j] = dp[i - 1][j - 1] + 1`；若不相等，则有`dp[i][j] = Math.max(dp[i][j - 1], dp[i - 1][j])`。
```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        if (text1 == null || text1.length() == 0 || text2 == null || text2.length() == 0)
            return 0;
        int m = text1.length(), n = text2.length();
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i][j - 1], dp[i - 1][j]);
                }
            }
        }
        return dp[m][n];
    }
}
```
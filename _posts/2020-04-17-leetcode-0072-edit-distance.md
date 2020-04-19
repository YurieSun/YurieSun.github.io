---
layout: post
title: LeetCode 0072 题解
description: "编辑距离"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[编辑距离](https://leetcode-cn.com/problems/edit-distance/)

### 题解
1. 法一（动态规划）：`dp[i][j]`的含义为，`word1`从0到i的子字符串变成`word2`从0到j的子字符串需要的最少操作数。Base case是当`i=0`或`j=0`时即两个子字符串分别为空的情况，此时填入另一子字符串长度即可，相当于不断增加字符。若`word1`和`word2`的当前字符相同，则不需要进行操作，即操作数不变；若不相同，则可以对`word1`的当前字符进行添加、删除、替换的操作，这三者中操作数较少的即为当前操作数。
```java
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length(), n = word2.length();
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 1; i <= m; i++)
            dp[i][0] = i;
        for (int i = 1; i <= n; i++)
            dp[0][i] = i;
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                // 由于数组dp[0]表示空字符串，因此字符是从1开始计数的，即dp[i]表示第i个字符，而在字符串中对应位置是i-1。
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    // 分别为删除、添加、替换
                    dp[i][j] = Math.min(Math.min(dp[i - 1][j], dp[i][j - 1]), dp[i - 1][j - 1]) + 1;
                }
            }
        }
        return dp[m][n];
    }
}
```
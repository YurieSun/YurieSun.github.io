---
layout: post
title: 剑指Offer 47 题解
description: "礼物的最大价值"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[礼物的最大价值](https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/)

### 题解
1. 法一（动态规划）：每一格的最大值等于它上面和左边格子中的较大值加上自己；初始状态为第一行和第一列，只能加上自己左边或上面的格子和自己。
```java
class Solution {
    public int maxValue(int[][] grid) {
        if (grid == null || grid.length == 0)
            return 0;
        int m = grid.length, n = grid[0].length;
        int[][] dp = new int[m][n];
        dp[0][0] = grid[0][0];
        for (int i = 1; i < m; i++)
            dp[i][0] = dp[i - 1][0] + grid[i][0];
        for (int i = 1; i < n; i++)
            dp[0][i] = dp[0][i - 1] + grid[0][i];
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]) + grid[i][j];
            }
        }
        return dp[m - 1][n - 1];
    }
}
```
* 改进：从状态方程可以看出每一个格子只与两个格子相关，则可以降低空间复杂度，采用一维数组存储状态。
```java
class Solution {
    public int maxValue(int[][] grid) {
        if (grid == null || grid.length == 0)
            return 0;
        int m = grid.length, n = grid[0].length;
        int[] dp = new int[n];
        // 通过grid的第一行对dp进行初始化
        for (int j = 0; j < n; j++) {
            if (j == 0)
                dp[j] = grid[0][0];
            else
                dp[j] = dp[j - 1] + grid[0][j];
        }
        // 更新dp时，dp[j-1]相当于它左边格子，dp[j]相当于它上面格子。
        for (int i = 1; i < m; i++)
            for (int j = 0; j < n; j++) {
                if (j == 0)
                    dp[j] = dp[j] + grid[i][j];
                else
                    dp[j] = Math.max(dp[j - 1], dp[j]) + grid[i][j];
            }
        return dp[n - 1];
    }
}
```
* 也可在原数组上存储计算的dp数组，即原地修改，但不建议修改输入。

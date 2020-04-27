---
layout: post
title: LeetCode 0474 题解
description: "一和零"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[一和零](https://leetcode-cn.com/problems/ones-and-zeroes/)

### 题解
1. 法一（动态规划）：这是一个多维费用的0-1背包问题。将该转化成如下的背包问题，即给定容量为`m`和容量为`n`的两个背包、以及`N`个物品（即字符串），每个物品的重量分别为当前字符串中`0`和`1`的个数，如何选择能使背包获得最大价值，即最大的字符串个数。因为是二维费用问题，因此需要使用三维数组。这里，`dp[i][j][k]`表示使用前`i`个字符串放入容量为`j`的背包和容量为`k`的背包中的最大价值。
```java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        int len = strs.length;
        int[][][] dp = new int[len + 1][m + 1][n + 1];
        // 初始化：空字符串以及背包容量为0时，价值为0。
        for (int i = 1; i <= len; i++) {
            for (int j = 0; j <= m; j++) {
                for (int k = 0; k <= n; k++) {
                    int[] temp = count(strs[i - 1]);
                    int zeros = temp[0];
                    int ones = temp[1];
                    if (j >= zeros && k >= ones) {
                        dp[i][j][k] = Math.max(dp[i - 1][j][k], dp[i - 1][j - zeros][k - ones] + 1);
                    } else {
                        dp[i][j][k] = dp[i - 1][j][k];
                    }
                }
            }
        }
        return dp[len][m][n];
    }
    public int[] count(String s) {
        int zeros = 0, ones = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '0') {
                zeros++;
            }
            if (s.charAt(i) == '1') {
                ones++;
            }
        }
        return new int[]{zeros, ones};
    }
}
```
* 空间压缩：使用二维数组。
```java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        int len = strs.length;
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 1; i <= len; i++) {
            for (int j = m; j >= 0; j--) {
                for (int k = n; k >= 0; k--) {
                    int[] temp = count(strs[i - 1]);
                    int zeros = temp[0];
                    int ones = temp[1];
                    if (j >= zeros && k >= ones) {
                        dp[j][k] = Math.max(dp[j][k], dp[j - zeros][k - ones] + 1);
                    }
                }
            }
        }
        return dp[m][n];
    }
    public int[] count(String s) {
        int zeros = 0, ones = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '0') {
                zeros++;
            }
            if (s.charAt(i) == '1') {
                ones++;
            }
        }
        return new int[]{zeros, ones};
    }
}
```
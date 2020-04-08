---
layout: post
title: 剑指Offer 60 题解
description: "n个骰子的点数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[n个骰子的点数](https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/)

### 题解
1. 法一（动态规划）：数组`dp[i][j]`表示用`i`个骰子掷出点数为`j`的情况数，则状态转移方程为 $dp[i][j]=\sum \limits_{k=1}^{6}dp[i-1][j-k]$。这是由于第`i`个骰子的点数为`1`到`6`，所以用`i`个骰子掷出点数为`j`的情况数就是用`i-1`个骰子分别掷出`j-1`, `j-2`, ..., `j-6`的情况总数。
```java
class Solution {
        public double[] twoSum(int n) {
            // 行、列都从1开始使用，则n个骰子掷出的点数范围为n到6n，因此数组长度分别加1。
            int[][] dp = new int[n + 1][6 * n + 1];
            // 初始状态，只有1个骰子时，掷出1到6只有一种情况。
            for (int i = 1; i <= 6; i++)
                dp[1][i] = 1;
            // 状态转移方程，注意i、j、k的起始和结束位置
            for (int i = 1; i <= n; i++) {
                for (int j = i; j <= 6 * i; j++) {
                    for (int k = 1; k <= 6; k++) {
                        if (j - k <= 0)
                            break;
                        dp[i][j] += dp[i - 1][j - k];
                    }
                }
            }
            // n到6n共有5n+1个数
            double[] res = new double[5 * n + 1];
            // 总次数为6的n次方
            double sum = Math.pow(6, n);
            for (int i = 0; i < 5 * n + 1; i++)
                res[i] = dp[n][i + n] / sum;
            return res;
        }
    }
```
---
layout: post
title: LeetCode 0416 题解
description: "分割等和子集"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)

### 题解
1. 法一（动态规划）：将其转化成如下的背包问题，即给定容量为`sum/2`的背包和`N`个物品，每个物品的重量为`nums[i]`，是否存在一种方案，使得背包恰好装满。这里，`dp[i][j]`表示使用前`i`件物品是否能将容量为`j`的背包装满。
```java
class Solution {
    public boolean canPartition(int[] nums) {
        if (nums == null || nums.length <= 1) {
            return false;
        }
        int sum = 0;
        // 求和
        for (int x : nums) {
            sum += x;
        }
        // 若和为奇数，肯定不存在。
        if ((sum & 1) != 0) {
            return false;
        }
        int N = nums.length;
        int W = sum / 2;
        boolean[][] dp = new boolean[N + 1][W + 1];
        // 初始化：dp[...][0] = true，即在背包容量为0的情况下肯定能装满；
        //         dp[0][...] = false，即若在没有物品的情况下肯定装不满。
        for (int i = 0; i <= N; i++) {
            dp[i][0] = true;
        }
        // 由于数组dp是从1开始表示数组nums中第0个物品的，因此这里nums下标取i-1。
        for (int i = 1; i <= N; i++) {
            for (int j = 1; j <= W; j++) {
                // 可以装下当前物品，就可选择装或不装。
                if (j >= nums[i - 1]) {
                    dp[i][j] = dp[i - 1][j] || dp[i - 1][j - nums[i - 1]];
                } 
                // 不能装下当前物品，只能不装。
                else {
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }
        return dp[N][W];
    }
}
```
* 也可以只初始化为`dp[0][0]=true`，然后从第1行第0个位置开始计算。
* 空间压缩：使用一维数组。
```java
class Solution {
    public boolean canPartition(int[] nums) {
        if (nums == null || nums.length <= 1) {
            return false;
        }
        int sum = 0;
        for (int n : nums) {
            sum += n;
        }
        if ((sum & 1) != 0) {
            return false;
        }
        int N = nums.length;
        int W = sum / 2;
        boolean[] dp = new boolean[W + 1];
        dp[0] = true;
        for (int i = 1; i < N; i++) {
            // 需要从后往前遍历，防止dp[j-nums[i-1]]被覆盖。
            for (int j = W; j >= 0; j--) {
                if (j >= nums[i - 1]) {
                    dp[j] = dp[j] || dp[j - nums[i - 1]];
                }
            }
        }
        return dp[W];
    }
}
```
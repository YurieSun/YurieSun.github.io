---
layout: post
title: LeetCode 0494 题解
description: "目标和"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[目标和](https://leetcode-cn.com/problems/target-sum/)

### 题解
1. 法一（DFS）:函数`dfs`中，`target`为目标值，`left`需要使用的数组下标。
```java
class Solution {
    public int findTargetSumWays(int[] nums, int S) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        return dfs(nums, S, 0);
    }
    public int dfs(int[] nums, int target, int left) {
        if (target == 0 && left == nums.length) {
            return 1;
        }
        if (left >= nums.length) {
            return 0;
        }
        int ans = 0;
        ans += dfs(nums, target + nums[left], left + 1);
        ans += dfs(nums, target - nums[left], left + 1);
        return ans;
    }
}
```
2. 法二（动态规划）：在给定数组中，假设从中选择作为正数的和为`x`，选择作为负数的绝对值的和为`y`，则有`x+y=sum`，`x-y=S`，可得`x=(sum+S)/2`。因此可将其转化成如下的背包问题，即给定容量为`(sum+S)/2`的背包和`N`个物品，每个物品的重量为`nums[i]`，使得背包恰好装满的方案有几种。这里，`dp[i][j]`表示使用前`i`件物品将容量为`j`的背包装满的方案个数。
```java
class Solution {
    public int findTargetSumWays(int[] nums, int S) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        int N = nums.length;
        int sum = 0;
        for (int x : nums) {
            sum += x;
        }
        if (((sum + S) & 1) != 0 || sum < S) {
            return 0;
        }
        int W = (sum + S) / 2;
        int[][] dp = new int[N + 1][W + 1];
        // 这里将第一行进行初始化：当没有物品时，只有在背包容量为0时存在一种能装满的方案，其它容量下不可能装满。
        dp[0][0] = 1;
        for (int i = 1; i <= N; i++) {
            for (int j = 0; j <= W; j++) {
                if (j >= nums[i - 1]) {
                    dp[i][j] = dp[i - 1][j] + dp[i - 1][j - nums[i - 1]];
                } else {
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }
        return dp[N][W];
    }
}
```
* 空间压缩：使用一维数组。
```java
class Solution {
    public int findTargetSumWays(int[] nums, int S) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        int sum = 0;
        for (int x : nums) {
            sum += x;
        }
        if (sum < S || ((sum + S) & 1) != 0) {
            return 0;
        }
        int N = nums.length;
        int W = (sum + S) / 2;
        int[] dp = new int[W + 1];
        dp[0] = 1;
        for (int i = 1; i <= N; i++) {
            for (int j = W; j >= 0; j--) {
                if (j >= nums[i - 1]) {
                    dp[j] += dp[j - nums[i - 1]];
                }
            }
        }
        return dp[W];
    }
}
```
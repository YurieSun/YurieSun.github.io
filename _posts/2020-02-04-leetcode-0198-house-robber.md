---
layout: post
title: LeetCode 0198 题解
description: "打家劫舍"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[打家劫舍](https://leetcode-cn.com/problems/house-robber/)

### 题解
1. 法一（动态规划）：通过股票系列问题得到的想法。对于每间房屋的状态，不抢设为0，抢设为1。则对于当前的房屋，`dp_i0`表示不抢，该状态对应的最大值应为前一间房屋中不抢`dp_i0`与抢`dp_i1`的较大值；`dp_i1`表示抢，该状态对应的最大值为前一间房屋不抢`dp_i0`并加上当前房屋的值。最终返回这两者的较大值即可。
```java
class Solution {
    public int rob(int[] nums) {
        int dp_i0 = 0, dp_i1 = 0;
        for(int i = 0; i < nums.length; i++){
            int temp = dp_i0;
            dp_i0 = Math.max(dp_i0, dp_i1);
            dp_i1 = temp + nums[i];
        }
        return Math.max(dp_i0, dp_i1);
    }
}
```
* 从状态方程上看，可写出状态方程为$f(n)=max(f(n-1),f(n-2)+num)$，由此可得到以下代码：
```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if(n == 0)
            return 0;
        int[] dp = new int[n + 1];
        dp[0] = 0;
        dp[1] = nums[0];
        for(int i = 2; i <= n; i++)
            dp[i] = Math.max(dp[i-1], dp[i-2] + nums[i-1]);
        return dp[n];
    }
}
```
* 由于每次计算只用到了前两个状态，因此可设置两个变量以压缩空间：
```java
class Solution {
    public int rob(int[] nums) {
        int preMax = 0, curMax = 0;
        for(int i : nums){
            int temp = curMax;
            curMax = Math.max(curMax, preMax + i);
            preMax = temp;
        }
        return curMax;
    }
}
```
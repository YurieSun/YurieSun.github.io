---
layout: post
title: LeetCode 0213 题解
description: "打家劫舍 II"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[打家劫舍 II](https://leetcode-cn.com/problems/house-robber-ii/)

### 题解
1. 法一（动态规划）：198题打家劫舍提供了计算从第1间到最后1间的方法。若房屋首尾相连，则只需比较两种情况下哪个获得的钱多：(1)抢第一家，则无需计算最后一家，`nums`从`0`计算到`n-2`即可；(2)不抢第一家，则`nums`从`1`计算到`n-1`。
```java
class Solution {
    public int rob(int[] nums) {
        if(nums.length == 1)
            return nums[0];
        return Math.max(myRob(nums, 0, nums.length-2), myRob(nums, 1, nums.length-1));
    }
    private int myRob(int[] nums, int start, int end){
        int dp_i0 = 0, dp_i1 = 0;
        for(int i = start; i <= end; i++){
            int temp = dp_i0;
            dp_i0 = Math.max(dp_i1, dp_i0);
            dp_i1 = temp + nums[i];
        }
        return Math.max(dp_i0, dp_i1);
    }
}
```
* 状态方程仍为$f(n)=max(f(n-1),f(n-2)+num)$：
```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if(n == 0)
            return 0;
        if(n == 1)
            return nums[0];
        int[] dp = new int[n + 1];
        //抢第一家
        dp[0] = 0;
        dp[1] = nums[0];
        for(int i = 2; i < n; i++)
            dp[i] = Math.max(dp[i-1], dp[i-2] + nums[i-1]); 
        int max1 = dp[n-1];
        //不抢第一家
        dp[0] = 0;
        dp[1] = 0;
        for(int i = 2; i <= n; i++)
            dp[i] = Math.max(dp[i-1], dp[i-2] + nums[i-1]);
        int max2 = dp[n];
        return Math.max(max1, max2);
    }
}
```
* 由于每次计算只用到了前两个状态，因此可设置两个变量以压缩空间：
```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if(n == 0)
            return 0;
        if(n == 1)
            return nums[0];
        //抢第一家
        int preMax = 0, curMax = nums[0];
        for(int i = 2; i < n; i++){
            int temp = curMax;
            curMax = Math.max(curMax, preMax + nums[i-1]);
            preMax = temp;
        }
        int max1 = curMax;
        //不抢第一家
        preMax = 0;
        curMax = 0;
        for(int i = 2; i <= n; i++){
            int temp = curMax;
            curMax = Math.max(curMax, preMax + nums[i-1]);
            preMax = temp;
        }
        int max2 = curMax;
        return Math.max(max1, max2);
    }
}
```
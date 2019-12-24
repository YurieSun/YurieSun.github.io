---
layout: post
title: LeetCode 0055 题解
description: "跳跃游戏"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[跳跃游戏](https://leetcode-cn.com/problems/jump-game/)

### 题解
1. 法一（回溯，超时）：递归列举所有路线。
```java
class Solution {
    public boolean canJump(int[] nums) {
        return canJumpFromtPosition(0, nums);
    }
    public boolean canJumpFromtPosition(int pos, int[] nums){
        if(pos == nums.length - 1)
            return true;
        int jump = Math.min(nums[pos] + pos, nums.length - 1);
        for(int i = pos + 1; i <= jump; i++){
            if(canJumpFromtPosition(i, nums))
                return true;
        }
        return false;
    }
}
```
2. 法二（动态规划）：数组`dp[]`存放从前面的位置能否跳跃到当前位置。根据前面的位置`j`来判断`i`是否能到达，若`j`可到达且从`j`可以到达的最大位置大于`i`，说明`i`可到达，将所有位置判断完毕后，根据数组`dp`的最后一项返回结果。
```java
class Solution {
    public boolean canJump(int[] nums) {
        boolean[] dp = new boolean[nums.length];
        dp[0] = true;
        for(int i = 1; i < nums.length; i++)
            for(int j = 0; j < i; j++)
                if(dp[j] && nums[j] + j >= i){
                    dp[i] = true;
                    break;
                }
        return dp[nums.length - 1];     
    }
}
```
3. 法三（贪心）：遍历数组，更新每个元可以到达的最远位置；若当前位置小于最远位置，说明到不了当前位置，直接返回`false`，否则，遍历完成说明可以到达最后一个位置，返回`true`。
```java
class Solution {
    public boolean canJump(int[] nums) {
        int max = 0;
        for(int i = 0; i < nums.length; i++){
            if(i > max)
                return false;
            max = Math.max(max, nums[i] + i);
        }
        return true;
    }
}
```
* 也可以从后往前跳，看是否能到达第一个位置
```java
class Solution {
    public boolean canJump(int[] nums) {
        if(nums.length < 2) return true;
        int start = nums.length - 2;
        int end = nums.length - 1;
        while(start >= 0){
            if(start + nums[start] >= end)
                end = start;
            start--;
        }
        return end == 0;
    }
}
```
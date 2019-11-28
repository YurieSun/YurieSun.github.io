---
layout: post
title: LeetCode 0674 题解
description: "最长连续递增数列"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[最长连续递增数列](https://leetcode-cn.com/problems/longest-continuous-increasing-subsequence/)

### 题解
1. 法一：扫描数组，当元素递增时增加计数器，若递减则将计数器重新初始化为1，并更新最大值。
```java
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        if(nums == null || nums.length == 0)
            return 0;
        int count = 1, max = 1;
        for(int i = 1; i < nums.length; i++){
            if(nums[i-1] < nums[i])
                count++;
            else
                count = 1;
            max = Math.max(max, count);
        }
        return max;
    }
}
```
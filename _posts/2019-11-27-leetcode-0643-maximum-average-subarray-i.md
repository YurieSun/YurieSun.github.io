---
layout: post
title: LeetCode 0643 题解
description: "子数组最大平均数 I"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[子数组最大平均数 I](https://leetcode-cn.com/problems/maximum-average-subarray-i/)

### 题解
1. 法一（滑动窗口法）：扫描数组，计算每`k`个连续元素的最大和，并更新最大值，扫描结束后返回平均值。
```java
class Solution {
    public double findMaxAverage(int[] nums, int k) {
        int sum = 0;
        for(int i = 0; i < k; i++)
            sum += nums[i];
        int max = sum;
        for(int i = k; i < nums.length; i++){
            sum += nums[i] - nums[i-k];
            max = Math.max(sum, max);
        }
        return (double)max/k;
    }
}
```
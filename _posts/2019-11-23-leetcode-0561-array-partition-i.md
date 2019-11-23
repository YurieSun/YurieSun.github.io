---
layout: post
title: LeetCode 0561 题解
description: "数组拆分 I"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[数组拆分 I](https://leetcode-cn.com/problems/array-partition-i/)

### 题解
1. 法一：根据题意，应该在数组排序后，按位置两两配对，那么将下标为偶数的元素相加即可。
```java
class Solution {
    public int arrayPairSum(int[] nums) {
        int sum = 0;
        Arrays.sort(nums);
        for(int i = 0; i < nums.length; i = i+2)
            sum += nums[i];
        return sum;
    }
}
```
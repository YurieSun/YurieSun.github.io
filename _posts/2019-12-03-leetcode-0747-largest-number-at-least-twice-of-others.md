---
layout: post
title: LeetCode 0747 题解
description: "至少是其他数字两倍的最大数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[至少是其他数字两倍的最大数](https://leetcode-cn.com/problems/largest-number-at-least-twice-of-others/)

### 题解
1. 法一：若要判断该数是否比其它数字两倍都更大，则只需判断这个数是否比第二大的数的两倍大。因此，遍历数组，找到最大的两个数，并记录最大数的下标，若满足条件则返回该下标，否则返回-1。
```java
class Solution {
    public int dominantIndex(int[] nums) {
        int first = Integer.MIN_VALUE, second = Integer.MIN_VALUE;
        int index = 0;
        for(int i = 0; i< nums.length; i++){
            if(nums[i] > first){
                second = first;
                first = nums[i];
                index = i;
            }
            else if(nums[i] > second)
                second = nums[i];
        }
        if(first >= second * 2)
            return index;
        return -1;
    }
}
```
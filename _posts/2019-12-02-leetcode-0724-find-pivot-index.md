---
layout: post
title: LeetCode 0724 题解
description: "寻找数组的中间索引"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[寻找数组的中间索引](https://leetcode-cn.com/problems/find-pivot-index/)

### 题解
1. 法一：先计算数组所有元素之和，再次扫描数组，若总和减去当前元素等于其左边所有元素的和的两倍，则说明当前元素为中间元素，否则，更新左边总和，并往下进行判断。
```java
class Solution {
    public int pivotIndex(int[] nums) {
        if(nums.length < 3) return -1;
        int sum = 0, leftSum = 0;
        for(int i = 0 ; i < nums.length; i++)
            sum += nums[i];
        for(int i = 0; i< nums.length; i++){
            if(sum - nums[i] == leftSum*2)
                return i;
            leftSum += nums[i];
        }
        return -1;
    }
}
```
* 注意：`if`中的判断条件不能带有除法，如`(sum - nums[i])/2 == leftSum`与上述方法中的判断条件看似等价，但`int`的除法会有截断，导致其计算结果不一样。
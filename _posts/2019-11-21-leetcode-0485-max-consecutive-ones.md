---
layout: post
title: LeetCode 0485 题解
description: "最大连续1的个数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[最大连续1的个数](https://leetcode-cn.com/problems/max-consecutive-ones/)

### 题解
1. 法一：依次扫描数组，若等于1，则进入循环并累计个数，不等于1时退出，同时更新最大个数。
```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int i = 0, max = 0, count = 0;
        while(i < nums.length){
            if(nums[i] == 1){
                int j = i;
                while(j < nums.length && nums[i] == nums[j]){
                    count++;
                    j++;
                }
                i = j;
            }
            i++;
            if(count > max)
                max = count;
            count = 0;
        }
        return max;
    }
}
```
* 稍简洁的写法
```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int max = 0, count = 0;
        for(int i = 0; i < nums.length; i++){
            if(nums[i] == 1)
                count++;   
            else
                count = 0;
            max = count > max ? count : max; 
        }
        return max; 
    }
}
```
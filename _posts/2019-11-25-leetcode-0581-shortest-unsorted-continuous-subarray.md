---
layout: post
title: LeetCode 0581 题解
description: "最短无序连续子数组"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[最短无序连续子数组](https://leetcode-cn.com/problems/shortest-unsorted-continuous-subarray/)

### 题解
1. 法一：根据题意，可复制数组，并进行排序；再对比两数组元素，元素不相等的第一个下标为所求子数组的起始位置，不相等的最后一个下标为所求子数组的结束位置。
```java
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int lo = nums.length, hi = 0;
        int[] aux = new int[nums.length];
        for(int i = 0; i < nums.length; i++)
            aux[i] = nums[i];
        Arrays.sort(aux);
        for(int i = 0; i < nums.length; i++){
            if(nums[i] != aux[i]){
                lo = Math.min(lo, i);
                hi = Math.max(hi, i);
            }
        }   
        return hi-lo+1 > 0 ? hi-lo+1 : 0;
    }
}
```

2. 法二：对数组进行两次扫描，第一次从左往右扫描，则比前面元素小（说明该元素放错了位置）的最大下标是所求子数组的结束位置；第二次从左往右扫描，则比后面元素大的最大下标是所求子数组的起始位置。
```java
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int lo = 0, hi = 0;
        int max = Integer.MIN_VALUE, min = Integer.MAX_VALUE;
        for(int i = 0; i < nums.length; i++){
            if(nums[i] > max)
                max = nums[i];
            else if(nums[i] < max)
                hi = i+1; 
                /*这里记录的是放错位置元素的下一个下标，是因为lo和hi都初始化为0；
                若这里是hi=i，则因返回hi-lo+1，但当数组递增时，会出现hi-lo+1=1而非返回0的情况。
                因此，在这里写成hi=i+1，使得返回变量可写成hi-lo，以避免这种错误情况。
                */
        }
        for(int i = nums.length - 1; i >= 0; i--){
            if(nums[i] < min)
                min = nums[i];
            else if(nums[i] > min)
                lo = i;
        }
        return hi-lo;
    }
}
```
* 或者将`lo`和`hi`的初始值改变，可写为：
```java
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int lo = nums.length, hi = 0;
        int max = Integer.MIN_VALUE, min = Integer.MAX_VALUE;
        for(int i = 0; i < nums.length; i++){
            if(nums[i] > max)
                max = nums[i];
            else if(nums[i] < max)
                hi = i;
        }
        for(int i = nums.length - 1; i >= 0; i--){
            if(nums[i] < min)
                min = nums[i];
            else if(nums[i] > min)
                lo = i;
        }
        return hi-lo+1 > 0 ? hi-lo+1 : 0;
    }
}
```
* 也可变成只扫描一遍：
```java
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int lo = nums.length, hi = 0;
        int max = Integer.MIN_VALUE, min = Integer.MAX_VALUE;
        for(int i = 0; i < nums.length; i++){
            if(nums[i] > max)
                max = nums[i];
            else if(nums[i] < max)
                hi = i;
            if(nums[nums.length-1-i] < min)
                min = nums[nums.length-1-i];
            else if(nums[nums.length-1-i] > min)
                lo = nums.length-1-i;
        }
        return hi-lo+1 > 0 ? hi-lo+1 : 0;
    }
}
```
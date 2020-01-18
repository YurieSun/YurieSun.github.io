---
layout: post
title: LeetCode 0327 题解
description: "区间和的个数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[区间和的个数](https://leetcode-cn.com/problems/count-of-range-sum/)

### 题解
1. 法一（暴力二重循环，超时）：遍历所有区间并计算它们的好是否符合条件。
```java
class Solution {
    public int countRangeSum(int[] nums, int lower, int upper) {
        int count = 0;
        for(int i = 0; i < nums.length; i++){
            long sum = 0;
            for(int j = i; j < nums.length; j++){
                sum += nums[j];
                if(sum >= lower && sum <= upper)
                    count++;
            }
        }
        return count;   
    }
}
```
2. 法二（归并排序）：若将数组中每一元素及其左边的和都存储在一个数组`sums`中，则区间[i, j]中元素的和等于`sums[j]-sums[i]`，因此题目要求就变为寻找有几对满足条件`lower <= sums[j] - sums[i] <= upper`且`i < j`的`i`与`j`。求逆序对是这个问题的特殊情况，可以通过在归并排序的同时进行计数来解决此问题。
```java
class Solution {
    public int countRangeSum(int[] nums, int lower, int upper) {
        long[] sums = new long[nums.length + 1];
        for(int i = 0; i < nums.length; i++)
            sums[i + 1] = sums[i] + nums[i];
        return countingMergeSort(sums, 0, sums.length, lower, upper);     
    }
    // 这里归并排序不包括start和end指向的元素。
    public int countingMergeSort(long[] nums, int start,int end, int lower, int upper){
        if(end - start <= 1) return 0;
        int mid = start + (end - start) / 2;
        int count = countingMergeSort(nums, start, mid, lower, upper) 
                  + countingMergeSort(nums, mid, end, lower, upper);
        long[] cache = new long[end - start];
        int j = mid, k = mid, t = mid;
        for(int i = start, r = 0; i < mid; i++, r++){
            // 从mid到end中满足<= upper的个数。
            while(j < end && nums[j] - nums[i] <= upper) j++;
            // 从mid到end中满足< lower的个数。
            while(k < end && nums[k] - nums[i] < lower) k++;
            // 数组cache是为了实现归并过程。
            while(t < end && nums[t] < nums[i]) cache[r++] = nums[t++];
            // 那么j-k就是满足lower <= sums[j] - sums[i] <= upper的个数。
            count += j - k;
            cache[r] = nums[i];
        }
        System.arraycopy(cache, 0, nums, start, t - start);
        return count;
    }
}
```
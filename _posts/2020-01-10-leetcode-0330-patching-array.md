---
layout: post
title: LeetCode 0330 题解
description: "按要求补齐数组"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[按要求补齐数组](https://leetcode-cn.com/problems/patching-array/)

### 题解
1. 法一（贪心）：设需要加入的最小数字为`miss`，则当前数字可表示的区间范围为$[1, miss)$；若将`miss`加入数组，则所表示区域拓展到$[1, 2\cdot miss)$。遍历数组时，若当前元素小于等于`miss`，说明该元素可用当前区间$[1, miss)$表示，同时将区间拓展为$[1, miss+nums[i])$；若当前元素大于`miss`时，说明该元素不可用当前区间表示，则需要将`miss`加入数组，并拓展区间为$[1, 2\cdot miss)$。
```java
class Solution {
    public int minPatches(int[] nums, int n) {
        long miss = 1;
        int count = 0, i = 0;
        while(miss <= n){
            // i < nums.length的控制应放在这而不能放在while循环中。
            if(i < nums.length && nums[i] <= miss)
                miss += nums[i++];
            else{
                miss += miss;
                count++;
            }
        }
        return count;
    }
}
```

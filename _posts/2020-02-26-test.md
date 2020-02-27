---
layout: post
title: LeetCode 0079 题解
description: "单词搜索"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[单词搜索](https://leetcode-cn.com/problems/word-search/)

### 题解
1. 法一（dfs）：对于每一个匹配的字符，对其上下左右进行搜索，查看是否有下一个字符，有则继续往下搜索，没有则回溯。
```java
class Solution {
    public int findMin(int[] nums) {
        if(nums == null || nums.length == 0)
            return -1;
        int left = 0, right = nums.length - 1;
        while(left < right){
            int mid = left + (right - left) / 2;
            if(nums[mid] > nums[right])
                left = mid + 1;
            else if(nums[mid] < nums[right])
                right = mid;
            else
                right--;
        }
        return nums[left];
    }
}
```
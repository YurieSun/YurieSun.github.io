---
layout: post
title: LeetCode 0154 题解
description: "寻找旋转排序数组中的最小值 II"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[寻找旋转排序数组中的最小值 II](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)

### 题解
1. 法一（二分法）：直接遍历数组找到最小值，时间复杂度为$O(n)$，使用二分法可将时间复杂度降为$O(\lg n)$。这道题与[153题](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)相似，只是本题中会出现重复元素。因此，在处理中间元素`nums[mid]`等于数组最右边的元素`nums[right]`的情况时，进行`right--`操作即可。首先`nums[right]`不可能是唯一的最小值，否则不会出现`nums[mid]==nums[right]`，因此，可将该元素删除，并继续循环。
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
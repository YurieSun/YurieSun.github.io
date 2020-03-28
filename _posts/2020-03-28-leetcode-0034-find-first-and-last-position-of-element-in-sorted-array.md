---
layout: post
title: LeetCode 0034 题解
description: "在排序数组中查找元素的第一个和最后一个位置"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

### 题解
1. 法一（二分）：通过二分寻找左右边界，注意细节。
```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        if (nums == null || nums.length == 0)
            return new int[]{-1, -1};
        int leftBound = 0, rightBound = 0;
        int left = 0, right = nums.length - 1;
        // 寻找左边界
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > target) {
                right = mid - 1;
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        if (left == nums.length || nums[left] != target)
            leftBound = -1;
        else
            leftBound = left;
        // 寻找右边界
        left = 0;
        right = nums.length - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > target) {
                right = mid - 1;
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                left = mid + 1;
            }
        }
        if (right < 0 || nums[right] != target)
            rightBound = -1;
        else
            rightBound = right;
        return new int[]{leftBound, rightBound};
    }
}
```
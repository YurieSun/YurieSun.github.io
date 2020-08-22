---
layout: post
title: LeetCode 0033 题解
description: "搜索旋转排序数组"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

### 题解
1. 法一（二分）
```java
class Solution {
    public int search(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return -1;
        }
        int left = 0, right = nums.length - 1;
        int cmp = nums[right];
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) {
                return mid;
            }
            // 比最右边元素小，说明mid在右半段有序数组中。
            if (nums[mid] <= cmp) {
                // 若target在mid到right的元素范围内，则在右半段找；否则在左半段找。
                if (target > nums[mid] && target <= nums[right]) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            } 
            // 比最右边元素大，说明mid在左半段有序数组中。
            else {
                if (target >= nums[left] && target < nums[mid]) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            }
        }
        return -1;
    }
}
```
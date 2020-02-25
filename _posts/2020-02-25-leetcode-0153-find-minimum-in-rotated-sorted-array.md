---
layout: post
title: LeetCode 0153 题解
description: "寻找旋转排序数组中的最小值"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)

### 题解
1. 法一（二分法）：直接遍历数组找到最小值，时间复杂度为$O(n)$，使用二分法可将时间复杂度降为$O(\lg n)$。具体做法为，若数组为旋转排序数组，则最小值左边的数值均比数组中最后一个数大，而从最小值开始的右侧有序序列均比最后一个数小；因此，通过二分法判断中间数是比最后一个数大还是小，即可确定最小数在哪。若中间数比最后一个数大，说明该数在最小数左侧，则移动左边界`left`；若中间数小于等于最后一个数，说明该数就在右侧的递增序列中，因此需要移动右边界。
```java
class Solution {
    public int findMin(int[] nums) {
        if(nums == null || nums.length == 0)
            return -1;
        int left = 0, right = nums.length - 1;
        int cmp = nums[right];
        while(left < right){
            int mid = left + (right - left) / 2;
            if(nums[mid] > cmp)
                left = mid + 1;
            else
                right = mid;
        }
        return nums[left];
    }
}
```
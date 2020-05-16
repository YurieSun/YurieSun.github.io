---
layout: post
title: LeetCode 0540 题解
description: "有序数组中的单一元素"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[有序数组中的单一元素](https://leetcode-cn.com/problems/single-element-in-a-sorted-array/)

### 题解
1. 法一（二分）：这个数组是排序的数组，假设单一元素的下标为`index`，那么在`index`前面的偶数下标`i`一定会满足`nums[i]=nums[i+1]`，而在`index`后面的偶数下标`i`一定不满足`nums[i]=nums[i+1]`。因此，可以根据`nums[i]`与`nums[i+1]`是否相等，来对区间进行二分。
```java
class Solution {
    public int singleNonDuplicate(int[] nums) {
        int left = 0, right = nums.length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            // 为了保证判断的下标为偶数
            if (mid % 2 == 1)
                mid--;
            // 若相等，说明index在mid+1后面，因此将left赋值为mid+2。
            if (nums[mid] == nums[mid + 1])
                left = mid + 2;
            // 若不相等，说明index在mid及其前面，因此将left赋值为mid。
            else
                right = mid;
        }
        return nums[left];
    }
}
```
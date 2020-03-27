---
layout: post
title: 剑指Offer 51 题解
description: "数组中的逆序对"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[数组中的逆序对](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

### 题解
1. 法一（归并排序）：在归并即`merge`的过程中，可以很快地计算出逆序对。这里是在数组右半部分的数更小的时刻进行统计，此时，左半部分还未进入辅助数组的数，都要比这个数大，即共有`mid-pl+1`个。若最后左半部分还有多的数，此时不需要统计，因为在右侧数字进入辅助数组的时刻已经统计过；若最后右半部分还有多的数，则这些数都比左半部分的数大，不会产生逆序对，因此也不需要统计。
```java
class Solution {
    public int reversePairs(int[] nums) {
        if (nums == null || nums.length < 2)
            return 0;
        return mergeSort(nums, 0, nums.length - 1);
    }
    public int mergeSort(int[] arr, int left, int right) {
        if (left == right)
            return 0;
        int mid = left + (right - left) / 2;
        return mergeSort(arr, left, mid) + mergeSort(arr, mid + 1, right) + merge(arr, left, mid, right);
    }
    public int merge(int[] arr, int left, int mid, int right) {
        int[] helper = new int[right - left + 1];
        int res = 0, index = 0;
        int pl = left, pr = mid + 1;
        while (pl <= mid && pr <= right) {
            // 这里的小于等于是为了计算有相同数字的情况，只有右边数字严格小于左边数字才进行统计。
            res += arr[pl] <= arr[pr] ? 0 : mid - pl + 1;
            helper[index++] = arr[pl] <= arr[pr] ? arr[pl++] : arr[pr++];

        }
        while (pr <= right) {
            helper[index++] = arr[pr++];
        }
        while (pl <= mid) {
            helper[index++] = arr[pl++];
        }
        for (int i = 0; i < helper.length; i++)
            arr[left + i] = helper[i];
        return res;
    }
}
```
---
layout: post
title: 剑指Offer 40 题解
description: "最小的k个数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

### 题解
1. 法一（排序）
```java
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        if (arr == null || arr.length == 0 || k <= 0)
            return new int[0];
        Arrays.sort(arr);
        return Arrays.copyOf(arr,k);
    }
}
```
2. 法二（快排）：在算法 4 的快排应用中，有找到第k小的数这一问题。
```java
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        if (arr == null || arr.length == 0 || k <= 0)
            return new int[0];
        int lo = 0, hi = arr.length - 1;
        while (lo < hi) {
            int cut = quicksort(arr, 0, arr.length - 1);
            if (cut < k)
                lo = cut + 1;
            else if (cut > k)
                hi = cut - 1;
            else
                break;
        }
        return  Arrays.copyOf(arr,k);
    }

    private int quicksort(int[] nums, int lo, int hi) {
        int i = lo, j = hi + 1;
        while (true) {
            while (nums[++i] < nums[lo])
                if (i == hi) break;
            while (nums[--j] > nums[lo])
                if (j == lo) break;
            if (i >= j)
                break;
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] = temp;

        }
        int temp = nums[lo];
        nums[lo] = nums[j];
        nums[j] = temp;
        return j;
    }
}
```
3. 法三（堆排序）
```java
class Solution {
    int N;
    public int[] getLeastNumbers(int[] arr, int k) {
        if (arr == null || arr.length == 0 || k <= 0)
            return new int[0];
        N = arr.length - 1;
        for (int i = (N - 1) / 2; i >= 0; i--)
            sink(arr, i, N);
        while (N > 0) {
            exchange(arr, 0, N--);
            sink(arr, 0, N);
        }
        return Arrays.copyOf(arr, k);
    }
    private void sink(int[] nums, int k, int N) {
        while (2 * k + 1 <= N) {
            int j = 2 * k + 1;
            if (j < N && nums[j] < nums[j + 1])
                j++;
            if (nums[k] >= nums[j])
                break;
            exchange(nums, k, j);
            k = j;
        }
    }
    private void exchange(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```
4. 法四（大顶堆）
```java
class Solution {
    int N;
    public int[] getLeastNumbers(int[] arr, int k) {
        if (arr == null || arr.length == 0 || k <= 0)
            return new int[0];
        int[] nums = new int[k];
        N = k - 1;
        for (int i = 0; i < k; i++)
            nums[i] = arr[i];
        // 用前k个数建立大顶堆
        for (int i = (N - 1) / 2; i >= 0; i--) {
            sink(nums, i);
        }
        // 后续数字中有比最大值更小的数，就将最大值替换掉，并进行sink操作。
        for (int i = k; i < arr.length; i++) {
            if (arr[i] < nums[0]) {
                nums[0] = arr[i];
                sink(nums, 0);
            }
        }
        return nums;
    }
    private void sink(int[] nums, int k) {
        while (2 * k + 1 <= N) {
            int j = 2 * k + 1;
            if (j < N && nums[j] < nums[j + 1])
                j++;
            if (nums[j] <= nums[k])
                break;
            exchange(nums, k, j);
            k = j;
        }
    }
    private void exchange(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```
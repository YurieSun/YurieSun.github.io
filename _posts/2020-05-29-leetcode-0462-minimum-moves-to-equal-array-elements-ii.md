---
layout: post
title: LeetCode 0462 题解
description: "最少移动次数使数组元素相等 II"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[最少移动次数使数组元素相等 II](https://leetcode-cn.com/problems/minimum-moves-to-equal-array-elements-ii/)

### 题解
1. 法一：若有两个数a、b (b>a) 需要移动最少次数到达同一个数，则这个数肯定在中间，假设为m，则需要走的步数为`(b-m)+(m-a)=b-a`，因此对于这两个数，走到中间任何一个数的步数都是一样的。那么对于一组数需要到达同一个数的情况，则需要让它们都到达中位数。第一种方法是对数组进行排序，分别从首尾开始组成一对，这两个数要变成中位数需要走的步数为`b-a`。因此，将每一对的步数相加，就是最终要走的总步数。
```java
class Solution {
    public int minMoves2(int[] nums) {
        Arrays.sort(nums);
        int i = 0, j = nums.length - 1;
        int res = 0;
        while (i < j) {
            res += nums[j] - nums[i];
            i++;
            j--;
        }
        return res;
    }
}
```
2. 法二（快速选择）：通过快速选择找到中位数。
```java
class Solution {
    public int minMoves2(int[] nums) {
        int res = 0;
        // 项数为奇数时，中位数是中间的数，其下标为length/2；
        // 项数为偶数时，中位数是中间两个数，下标为length/2的数对应的是后一个数。
        int mid = quickSearch(nums, nums.length / 2);
        for (int x : nums) {
            res += Math.abs(x - mid);
        }
        return res;
    }
    // 返回的是nums[k]，其实是第k+1大的元素。
    private int quickSelect(int[] nums, int k) {
        int lo = 0, hi = nums.length - 1;
        while (lo < hi) {
            int m = partition(nums, lo, hi);
            if (m == k)
                break;
            else if (m > k)
                hi = m - 1;
            else
                lo = m + 1;
        }
        return nums[k];
    }
    private int partition(int[] nums, int lo, int hi) {
        int i = lo, j = hi + 1;
        while (true) {
            while (nums[++i] < nums[lo] && i < hi) ;
            while (nums[--j] > nums[lo] && j > lo) ;
            if (i >= j)
                break;
            swap(nums, i, j);
        }
        swap(nums, lo, j);
        return j;
    }
    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```
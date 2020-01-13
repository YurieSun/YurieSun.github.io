---
layout: post
title: LeetCode 0321 题解
description: "拼接最大数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[拼接最大数](https://leetcode-cn.com/problems/create-maximum-number/)

### 题解
1. 法一（贪心+动态规划）：首先解决两个子问题【寻找一个数组中长度为k的最大子序列】和【对两个数组进行归并，使得归并后的数组所表示的数字最大】，那么这道题相当于列举出所有划分情况，找到每种情况下两个数组中对应的最大子序列，然后进行归并，最后返回所有情况中表示数字最大的数组。
* 子问题一（贪心）：寻找一个数组中长度为 $k$ 的最大子序列。首先创建长度为`k`的数组来存储这个最大子序列，之后扫描数组，在当前子序列中找到比当前元素大的元素`num`，则抛弃后面较小的元素，将当前元素插入到`num`后面，直至扫描数组结束或者所抛弃的元素达到上限。
* 子问题二（贪心）：对两个数组进行归并，使得归并后的数组所表示的数字最大。按照常用的归并算法即可。
* 主函数（动态规划）：对所有情况进行列举，并得到最大值。
```java
class Solution {
    public int[] maxNumber(int[] nums1, int[] nums2, int k) {
        int m = nums1.length, n = nums2.length;
        int[] max = new int[k];
        // k>m与k>n的情况均有可能。
        // 若k>n，则至少要在nums1中取k-n个元素，此时i的起始值应为k-n，所以i的起始条件为i=Math.max(0,k-n)；
        // 若k>m，则i<=k会导致nums2中下标溢出，所以终止条件为i<=k && i<=m。
        for(int i = Math.max(0, k - n); i <= k && i <= m; i++){
            int[] temp = merge(maxNumber(nums1, i), maxNumber(nums2, k - i));
            // 这里用到辅助函数，两个数组从第0个位置开始比较大小。
            if(greater(temp, 0, max, 0))
                max = temp;
        }
        return max;      
    }
    // 子问题一：寻找一个数组中长度为k的最大子序列。
    public int[] maxNumber(int[] nums, int k){
        int[] ans = new int[k];
        int n = nums.length;
        for(int i = 0, j = 0; i < n; i++){
            // n-i+j>k的含义：n-i为nums剩下的数字个数，j为ans中存在的数字个数，那么它们之和一定要大于k，否则数字个数不够，无法得到长度为k的子序列。
            // j>0的含义：由于是从末尾不断递减指针，必须保证指针不越界。
            // ans[j-1]<nums[i]的含义：若当前元素大，则一直循环，直到找到比当前元素小的位置，即为正确位置。
            while(n - i + j > k && j > 0 && ans[j - 1] < nums[i])
                j--;
            if(j < k)
                ans[j++] = nums[i];
        }
        return ans;
    }
    // 子问题二：对两个数组进行归并，使得归并后的数组所表示的数字最大。
    public int[] merge(int[] nums1, int[] nums2){
        int m = nums1.length, n = nums2.length;
        int[] ans = new int[m + n];
        int i = 0, j = 0, p = 0;
        while(p < m + n){
            if(greater(nums1, i, nums2, j))
                ans[p++] = nums1[i++];
            else
                ans[p++] = nums2[j++];
        }
        return ans;
    }
    // 辅助函数：比较大小，主要用在主函数中比较两数组所表示的数字的大小；
    // 若高位相等，则依次比较。
    public boolean greater(int[] nums1, int i, int[] nums2, int j){
        while(i < nums1.length && j < nums2.length && nums1[i] == nums2[j]){
            i++;
            j++;
        }
        return (j == nums2.length) || (i < nums1.length && nums1[i] > nums2[j]);
    }
}
```
---
layout: post
title: LeetCode 0315 题解
description: "计算右侧小于当前元素的个数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[计算右侧小于当前元素的个数](https://leetcode-cn.com/problems/count-of-smaller-numbers-after-self/)

### 题解
1. 法一（归并排序）：该问题是求逆序对的个数。在归并排序的同时，计算对应元素的逆序对个数。归并排序时元素已经被打乱，因此需要通过归并索引数组来解决。
```java
class Solution {
    private int[] index;
    private int[] counts;
    private int[] aux;
    public List<Integer> countSmaller(int[] nums) {
        List<Integer> ans = new ArrayList<>();
        int len = nums.length;
        if(len == 0) 
            return ans;
        index = new int[len];
        counts = new int[len];
        aux = new int[len];
        for(int i = 0; i < len; i++)
            index[i] = i;
        countingMergeSort(nums, 0, len - 1);       
        for(int count : counts)
            ans.add(count);
        return ans;
    }
    // 排序过程。
    public void countingMergeSort(int[] nums, int start, int end){
        if(end <= start) return ;
        int mid = start + (end - start) / 2;
        countingMergeSort(nums, start, mid);
        countingMergeSort(nums, mid + 1, end);
        // 若左右数组无序时才进行归并。
        if(nums[index[mid]] > nums[index[mid + 1]])
            countingMerge(nums, start, end, mid);
    }
    // 归并过程。在左侧元素出列时计算次数，右侧元素出列则无需计数。
    public void countingMerge(int[] nums, int start, int end, int mid){
        for(int i = start; i <= end; i++)
            aux[i] = index[i];
        int i = start, j = mid + 1, k = start;
        while(k <= end){
            if(i > mid)
                index[k++] = aux[j++];
            // 若右侧元素已全部出列，意味着它们比当前的左侧元素都要小，个数均为end - mid。
            else if(j > end){
                index[k] = aux[i++];
                counts[index[k]] += end - mid;
                k++;
            }   
            // 当左侧元素出列时，计算次数为j - mid - 1。
            else if(nums[aux[i]] <= nums[aux[j]]){
                index[k] = aux[i++];
                counts[index[k]] += j - mid - 1;
                k++;
            }
            else   
                index[k++] = aux[j++];
        }
    }
}
```
* 另一种写法（可与[剑指Offer 51题 数组中的逆序对](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)对比着看）：
```java
class Solution {
    private int[] cnt;
    private int[] index;
    public List<Integer> countSmaller(int[] nums) {
        List<Integer> list = new ArrayList<>();
        if (nums == null || nums.length == 0)
            return list;
        cnt = new int[nums.length];
        index = new int[nums.length];
        for (int i = 0; i < nums.length; i++)
            index[i] = i;
        mergeSort(nums, 0, nums.length - 1);
        for (int c : cnt)
            list.add(c);
        return list;
    }
    public void mergeSort(int[] arr, int left, int right) {
        if (left == right)
            return;
        int mid = left + (right - left) / 2;
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }
    public void merge(int[] arr, int left, int mid, int right) {
        int pl = left, pr = mid + 1;
        int i = 0;
        int[] helper = new int[right - left + 1];
        while (pl <= mid && pr <= right) {
            // 从样例发现相等元素也要算进去，因此有等号。
            if (arr[index[pl]] <= arr[index[pr]])
                cnt[index[pl]] += pr - mid - 1;
            helper[i++] = arr[index[pl]] <= arr[index[pr]] ? index[pl++] : index[pr++];
        }
        while (pl <= mid) {
            cnt[index[pl]] += right - mid;
            helper[i++] = index[pl++];
        }
        while (pr <= right) {
            helper[i++] = index[pr++];
        }
        for (int j = 0; j < helper.length; j++)
            index[left + j] = helper[j];
    }
}
```
2. 其余方法待学习。
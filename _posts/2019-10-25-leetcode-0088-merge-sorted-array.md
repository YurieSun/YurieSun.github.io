---
layout: post
title: LeetCode 0088 题解
description: "合并两个有序数组"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)

### 思路
将排序后的结果存到新建数组中，比较两数组中数的大小，将较小值存入新数组中，直到某一数组扫描结束，再将另一数组剩余元素全部复制过去。（这是`mergesort`中`merge`的做法）

### 题解
```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int[] res = new int[m+n];
    	int i = 0, j = 0;
        for(int k = 0; k < m+n; k++) {
        	if(i > m-1) res[k] = nums2[j++];
        	else if(j > n-1) res[k] = nums1[i++];
        	else if(nums1[i] > nums2[j])
        		res[k] = nums2[j++];
        	else
        		res[k] = nums1[i++];
        }
        for (int t = 0; t < m+n; t++)
        	nums1[t] = res[t];
    }
}
```
* 改进一：现将`nums1`复制到新数组中，比将排序后的数组复制回`nums1`去，会少复制一些元素；同时可使用库函数`arraycopy(arr1, i, arr2, j, n)`进行数组的复制（表示将$arr1$中从第$i$个元素开始，复制到$arr2$中从第$j$个位置，复制元素个数为$n$）。
```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int[] aux = new int[m];
        System.arraycopy(nums1, 0, aux, 0, m);
    	int i = 0, j = 0;
        for(int k = 0; k < m+n; k++) {
        	if(i > m-1) nums1[k] = nums2[j++];
        	else if(j > n-1) nums1[k] = aux[i++];
        	else if(aux[i] > nums2[j])
        		nums1[k] = nums2[j++];
        	else
        		nums1[k] = aux[i++];
        }
    }
}
```
* 改进二：从后往前更新`nums1`的元素，这样在开头可以不需要复制`nums1`的原始元素，节约空间，但时间复杂度不变。
```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int i = m-1, j = n-1;
        for(int k = m+n-1; k >= 0; k--) {
        	if(i < 0) nums1[k] = nums2[j--];
        	else if(j < 0) nums1[k] = nums1[i--];
        	else if(nums1[i] > nums2[j])
        		nums1[k] = nums1[i--];
        	else
        		nums1[k] = nums2[j--];
        }
    }
}
```
* 注意  
从后往前进行更新其实不需要判断循环结束后，没有遍历完的是`nums1`还是`nums2`：若为`nums1`，则较小元素已按顺序放在了它的位置上，因此循环结束后，只需将`nums2`复制到`nums1`中去。但若删去第二个循环分支`else if(j < 0) nums1[k] = nums1[i--];`，则会因为`nums2`为空时报错，因此并未删除。

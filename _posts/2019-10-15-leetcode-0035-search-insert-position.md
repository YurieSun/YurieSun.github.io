---
layout: post
title: LeetCode 0035 题解
description: "搜索插入位置"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[实现strStr](https://leetcode-cn.com/problems/search-insert-position/)

### 思路
二分查找

### 题解

```java
class Solution {
    public static int searchInsert(int[] nums, int target) {
	    	int N = nums.length;
	    	int lo = 0, mid, hi = N-1; 
	    	while(lo <= hi) {
	    		mid = (lo + hi) / 2;
	    		if(target < nums[mid]) hi = mid - 1;
	    		else if(target > nums[mid]) lo = mid + 1;
	    		else return mid;
	    	}
	    	return lo;
	    }
}
```

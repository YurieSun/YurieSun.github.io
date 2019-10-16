---
layout: post
title: LeetCode 0035 题解
description: "搜索插入位置"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

### 思路
二分查找

### 题解

```java
class Solution {
    public static int searchInsert(int[] nums, int target) {
	    	int N = nums.length;
	    	int lo = 0, mid, hi = N-1; 
	    	while(lo <= hi) {
	    		mid = lo + (hi - lo) / 2;
	    		if(target < nums[mid]) hi = mid - 1;
	    		else if(target > nums[mid]) lo = mid + 1;
	    		else return mid;
	    	}
	    	return lo;
	    }
}
```
* 注意  
计算`mid`不要使用`mid = (lo + hi) / 2`，当`lo`和`hi`很大时，会出现整型溢出，因此采用`mid = lo + (hi - lo) / 2`或者使用无符号右移`mid = (lo + hi) >>> 1`。

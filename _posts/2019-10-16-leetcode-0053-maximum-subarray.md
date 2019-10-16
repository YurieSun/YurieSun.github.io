---
layout: post
title: LeetCode 0053 题解
description: "最大子序和"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

### 思路
在线处理：若前面的序列使和大于0，则加入`nums[i]`中，否则抛弃之，只保留当前项`nums[i]`，并更新最大和。

### 题解

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int thissum = 0, maxsum = nums[0];
        for(int i = 0; i < nums.length; i++){
            if(thissum > 0) 
                thissum += nums[i];
            else
                thissum = nums[i];
            maxsum = Math.max(thissum, maxsum);
        }
        return maxsum;
    }
}
```
* 法二：分而治之  
```java
class Solution {
    public static int maxSubArray(int[] nums) {
        return maxSubArray(nums, 0, nums.length - 1);
    }
	public static int max3(int a, int b, int c) {
		if(a >= b && a >= c) return a;
		if(b >= a && b >= c) return b;
		return c;
	}
	public static int maxSubArray(int[] nums, int lo, int hi) {
        if(lo == hi) return nums[lo];
        int mid = lo + (hi - lo) / 2;
        // 左子列最大和
        int maxleft = maxSubArray(nums, lo, mid);
        // 右子列最大和
        int maxright = maxSubArray(nums, mid+1, hi);
        // 跨越边界的最大和
        int sumleft = nums[mid], sumright = nums[mid+1];
        int thissum = sumleft;
        for (int i = mid-1; i >= lo; i--) {
        	thissum += nums[i];
        	if(thissum > sumleft)
        		sumleft = thissum;
        }
        thissum = sumright;
        for (int i = mid+2; i <= hi; i++) {
        	thissum += nums[i];
        	if(thissum > sumright)
        		sumright = thissum;
        }
        int maxmid = sumleft + sumright;
		return max3(maxleft, maxright, maxmid);
	}
}
```

### 思考
在线处理是一个很好的方法，其时间与$N$成正比，实际上是动态规划的问题；而分治法则是典型的递归，与mergesort有相似之处。
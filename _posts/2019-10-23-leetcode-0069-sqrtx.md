---
layout: post
title: LeetCode 0069 题解
description: "x的平方根"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[x的平方根](https://leetcode-cn.com/problems/sqrtx/)

### 思路
二分查找，从0找到$\frac{x}{2}$，若该数的平方小于$x$，则查找右半部分，反之查找左半部分。

### 题解

```java
class Solution {
    public int mySqrt(int x) {
        long lo = 0, hi = x/2 + 1;
	     while(lo < hi) {
	    	 long mid = lo + (hi - lo + 1)/2;
	    	 if(mid * mid > x) hi = mid - 1;
	    	 else if (mid * mid < x) lo = mid;
	    	 else return (int)mid;
	     }
	     return (int)lo;
    }
}
```
* 注意  
1. 一般来说一个数会小于等于该数一半的平方，因此可以只找$x$的前半部分；经过计算可知，当$(\frac{x}{2})^2 \lt x$时，$x$的取值可为0、1、2、3，它们的平方根均为1。因此应将右边界设为`hi = x/2 + 1`。  
2. 二分查找时还要注意中位数的选取，可取左中位数还是右中位数，还是都能取，还是都不取？这决定了`mid`的取值。若可取左中位数，则`mid = lo + (hi - lo)/2`，若可取左中位数，则`mid = lo + (hi - lo + 1)/2`。同时要注意更新`lo`与`hi`时能否取`mid`。特别要注意避免死循环。  
3. 本题中若数字很大，计算`mid * mid`时容易溢出，因此先按照`long`型计算，返回时再做强制转换。

### 思考
在二分查找时，通常需要考虑最后的返回值，中位数的选取，每个分支中左边界与右边界的选择等问题。这个总结“[十分好用的二分查找法模板](https://leetcode-cn.com/problems/search-insert-position/solution/te-bie-hao-yong-de-er-fen-cha-fa-fa-mo-ban-python-/)”对二分查找的问题很有帮助。
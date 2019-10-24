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
二分查找，从0找到$x$，若该数的平方大于$x$，则查找左半部分，反之查找右半部分。

### 题解

```java
class Solution {
    public int mySqrt(int x) {
        long lo = 0, hi = x;
	    while(lo < hi) {
            long mid = (lo + hi + 1) >>> 1;
            if(mid * mid > x) hi = mid - 1;
            else lo = mid;
        }
        return (int)lo;
    }
}
```
* 注意    
1. 二分查找时还要注意中位数的选取，可取左中位数还是右中位数，还是都能取，还是都不取？这决定了`mid`的取值。特别要注意避免死循环，若可取右中位数，则`mid = lo + (hi - lo)/2`，若可取左中位数，则`mid = lo + (hi - lo + 1)/2`；即左边界不收缩时应取右中位数，右边界不收缩时应取左中位数。  
2. 本题中若数字很大，计算`mid * mid`时容易溢出，因此先按照`long`型计算，返回时再做强制转换。

* 法二：牛顿迭代法
```java
class Solution {
    public int mySqrt(int x) {
        long a = x;
        while(a * a > x) {
            a = (a + x/a)/2;
        }
        return (int)a;
    }
}
```

### 思考
在二分查找时，通常需要考虑最后的返回值，中位数的选取，每个分支中左边界与右边界的选择等问题。这个总结“[十分好用的二分查找法模板](https://leetcode-cn.com/problems/search-insert-position/solution/te-bie-hao-yong-de-er-fen-cha-fa-fa-mo-ban-python-/)”对二分查找的问题很有帮助。
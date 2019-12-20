---
layout: post
title: LeetCode 0275 题解
description: "H指数 II"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[H指数 II](https://leetcode-cn.com/problems/h-index-ii/)

### 题解
1. 法一：若数组已经有序，则直接采用二分法。
```java
class Solution {
    public int hIndex(int[] citations) {
        if(citations == null || citations.length == 0)
            return 0;
        int N = citations.length;
        int left = 0, right = N;
        while(left < right){
            int mid = left + (right - left)/2;
            if(citations[mid] < N-mid)
                left = mid + 1;
            else
                right = mid;
        }
        return N-left;
    }
}
```

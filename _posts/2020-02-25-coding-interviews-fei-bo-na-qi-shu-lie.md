---
layout: post
title: 剑指Offer 10- I 题解
description: "斐波那契数列"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

### 题解
1. 法一：这道题可用递归、使用备忘录的动态规划、以及使用两个变量的动态规划三种方法。由于`n`太大时，递归容易超时，因此，不采用递归。同时，在`n`很大时，导致计算出来的数值很大，如果只对最后的返回值取余，那么在计算过程中就会造成溢出。因此，在每次循环中，都对结果进行取余，防止溢出发生。因为有$(a+b)\%k=(a\%k+b\%k)\%k$，所以这种取余操作是可行的。
```java
class Solution {
    public int fib(int n) {
        if(n == 0)
            return 0;
        if(n == 1)
            return 1;
        int first = 0, second = 1;
        int sum = 0;
        for(int i = 2; i <= n; i++){
            res = (first + second) % 1000000007;
            first = second;
            second = res;
        }
        return res;
    }
}
```
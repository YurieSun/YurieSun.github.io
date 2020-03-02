---
layout: post
title: LeetCode 0050 题解
description: "Pow(x, n)"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[Pow(x, n)](https://leetcode-cn.com/problems/powx-n/)

### 题解
1. 法一：暴力求解会超时。这里采用快速幂法：若`n`为偶数，有$x^n=(x^{\frac{n}{2}})^2$；若`n`为奇数，有$x^n=(x^{\frac{n}{2}})^2\times x$。采用递归时间，时间复杂度降为$O(\lg n)$，空间复杂度为$O(\lg n)$。
```java
class Solution {
    public double myPow(double x, int n) {
        long N = n;
        if(N < 0){
            x = 1 / x;
            N = -N;
        }
        return fastPow(x, N);
    }
    private double fastPow(double x, long n){
        if(n == 0)
            return 1;
        double half = fastPow(x, n / 2);
        if(n % 2 == 0)
            return half * half;
        else
            return half * half * x;
    }
}
```
* 也可使用循环实现快速幂算法。
```java
class Solution {
    public double myPow(double x, int n) {
        long N = n;
        if(N < 0){
            x = 1 / x;
            N = -N;
        }
        double res = 1;
        while(N > 0){
            if(N % 2 == 1)
                res = res * x;
            x = x * x;
            N /= 2;
        }
        return res;
    }
}
```
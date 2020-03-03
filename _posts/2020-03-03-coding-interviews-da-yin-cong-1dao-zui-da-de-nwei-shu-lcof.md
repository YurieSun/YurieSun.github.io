---
layout: post
title: 剑指Offer 17 题解
description: "打印从1到最大的n位数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[打印从1到最大的n位数](https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/)

### 题解
1. 法一：根据排列组合可得从1到最大的n位数共有$10^n-1$个，因此直接使用库函数算指数。
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
* 同样，可以使用快速幂方法计算指数。
```java
class Solution {
    public int[] printNumbers(int n) {
        int len = (int)fastPow(10, n) - 1;
        int[] res = new int[len];
        for(int i = 0; i < len; i++){
            res[i] = i + 1;
        }
        return res;
    }
    private double fastPow(double x, int n){
        double res = 1;
        while(n > 0){
            if((n & 1) != 0)
                res = res * x;
            x = x * x;
            n /= 2;
        }
        return res;
    }
}
```
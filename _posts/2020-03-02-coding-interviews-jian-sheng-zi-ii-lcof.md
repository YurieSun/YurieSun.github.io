---
layout: post
title: 剑指Offer 14-II 题解
description: "剪绳子 II"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[剪绳子 II](https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof/)

### 题解
1. 法一：和Leetcode [0343 整数拆分](https://leetcode-cn.com/problems/integer-break/)一样，只不过计算结果太大，需要解决大数求余的问题。第一种是循环求余，即每做一次乘法，都进行取余操作。时间复杂度为$O(n)$。
```java
class Solution {
    public int cuttingRope(int n) {
        if(n <= 3)
            return n - 1;
        int a = n / 3,b = n % 3;
        long res =0;
        if(b == 0)
            res = helper(3, a, 1000000007);
        if(b == 1)
            res = helper(3, a-1, 1000000007) * 4;
        if(b == 2)
            res = helper(3, a, 1000000007) * 2;
        return (int)(res % 1000000007);
    }
    private long helper(int x,int a,int p){
        long res = 1;
        while(a-- > 0)
            res = res * x % p;
        return res;
    }
}
```
* 改进：使用快速幂求余可以将时间复杂度降为$O(\lg n)$。其具体做法为：当$a$为偶数时，$x^a\%p=(x^2\%p)^{\lfloor a/2 \rfloor}\%p$；当$a$为奇数时，$x^a\%p=(x(x^2\%p)^{\lfloor a/2 \rfloor})\%p$。
```java
class Solution {
    public int cuttingRope(int n) {
        if(n <= 3)
            return n - 1;
        int a = n / 3,b = n % 3;
        long res =0;
        if(b == 0)
            res = helper(3, a, 1000000007);
        if(b == 1)
            res = helper(3, a-1, 1000000007) * 4;
        if(b == 2)
            res = helper(3, a, 1000000007) * 2;
        return (int)(res % 1000000007);
    }
    private long helper(long x,int a,int p){
        long res = 1;
        while(a > 0){
            if(a % 2 == 1)
                res = (res * x) % p;
            x= (x * x) % p;
            a /= 2;
        }
        return res;
    }
}
```
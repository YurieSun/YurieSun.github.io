---
layout: post
title: LeetCode 0371 题解
description: "两整数之和"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[两整数之和](https://leetcode-cn.com/problems/sum-of-two-integers/)

### 题解
1. 法一（位运算）：对于`a+b`，可以分成两部分看：`a^b`是两个数相加忽略进位时的结果；`a&b`可以得到两个数在哪些位上会进位，再将其左移一位`(a&b)<<1`，和未进位结果相加就是最终答案。即`a+b = (a^b) + ((a&b)<<1)`，但此时又出现了加法，因此需要不断循环，直到没有进位为止。
```java
class Solution {
    public int getSum(int a, int b) {
        while (b != 0) {
            int temp = a ^ b;
            b = (a & b) << 1;
            a = temp;
        }
        return a;
    }
}
```
* 也可以采用递归的方式，更好理解。
```java
class Solution {
    public int getSum(int a, int b) {
        if (b == 0)
            return a;
        return getSum(a ^ b, (a & b) << 1);
    }
}
```
---
layout: post
title: LeetCode 0400 题解
description: "第N个数字"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[第N个数字](https://leetcode-cn.com/problems/nth-digit/)

### 题解
1. 法一（数学）：先找到是几位数，再找是该位数上的哪个数字，最后找是这个数字的第几位。
```java
class Solution {
    public int findNthDigit(int n) {
        if (n < 0)
            return -1;
        long place = 1;
        long base = 1;
        while (n > 9 * base * place) {
            n -= 9 * base * place;
            base *= 10;
            place++;
        }
        String num = (int) (base + (n - 1) / place) + "";
        int index = (int) ((n - 1) % place);
        return num.charAt(index) - '0';
    }
}
```
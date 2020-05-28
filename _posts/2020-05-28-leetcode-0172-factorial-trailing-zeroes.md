---
layout: post
title: LeetCode 0172 题解
description: "阶乘后的零"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[阶乘后的零](https://leetcode-cn.com/problems/factorial-trailing-zeroes/)

### 题解
1. 法一：如果直接计算阶乘后再统计末尾0的个数，很容易产生溢出。这里的思路是，`0`只能由`2x5`得到，且`2`的个数要多于`5`的个数，因此能找到一个`5`必定会有一个`2`与之匹配产生`0`，因此只需统计从`1`到`n`这些数里总共有多少个`5`即可。由于`5`是每隔5个数出现一次的，因此`n/5`可以得到5的个数；而`25`可以产生两个`5`，且每隔`25`个数出现一次，因此`n/25`可以得到25的个数，以此类推。这样，从`1`到`n`这些数里总共的`5`的个数就是`n/5 + n/25 + n/125 + ...`。
```java
class Solution {
    public int trailingZeroes(int n) {
        if (n <= 0)
            return 0;
        int res = 0;
        while (n != 0) {
            res += n / 5;
            n /= 5;
        }
        return res;
    }
}
```
* 递归版本
```java
class Solution {
    public int trailingZeroes(int n) {
        if (n == 0)
            return 0;
        return trailingZeroes(n / 5) + n / 5;
    }
}
```
* 拓展：若需要计算`N!`的二进制表示中最低位`1`的位置，只要统计有多少个`2`即可，这和求解有多少个`5`思路一样，详见编程之美：2.2。
---
layout: post
title: LeetCode 0461 题解
description: "汉明距离"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[汉明距离](https://leetcode-cn.com/problems/hamming-distance/)

### 题解
1. 法一：将两个数进行异或，再求得到的值中1的个数即可。
```java
class Solution {
    public int hammingDistance(int x, int y) {
        int z = x ^ y;
        int res = 0;
        while (z != 0) {
            if ((z & 1) == 1) {
                res++;
            }
            z >>= 1;
        }
        return res;
    }
}
```
* 改进：可以不用逐位判断，而是利用$n\&(n-1)$将$n$的最低位的1置为0，这样可以减少循环次数。
```java
class Solution {
    public int hammingDistance(int x, int y) {
        int z = x ^ y;
        int res = 0;
        while (z != 0) {
            res++;
            z &= (z - 1);
        }
        return res;
    }
}
```
* 也可以使用Java内置函数`Integer.bitCount()`来统计 1 个的个数。
```java
class Solution {
    public int hammingDistance(int x, int y) {
        return Integer.bitCount(x ^ y);
    }
}
```
---
layout: post
title: LeetCode 0190 题解
description: "颠倒二进制位"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[颠倒二进制位](https://leetcode-cn.com/problems/reverse-bits/)

### 题解
1. 法一（位运算）
```java
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        int res = 0;
        for (int i = 0; i < 32; i++) {
            // res左移一位，为新放入的位开辟空间
            res <<= 1;
            // (n & 1)获取n的最低位，并放到res的最低位
            res |= (n & 1);
            // n右移，将已经复制到res的位舍去
            n >>= 1;
        }
        return res;
    }
}
```
* 改进：如果需要多次使用该方法，可以以字节为单位（8 bit）进行翻转，并加入缓存，以提高速度。
```java
public class Solution {
    Map<Byte, Integer> cache = new HashMap();
    public int reverseBits(int n) {
        int res = 0;
        // 以字节为单位翻转，因此把32位整型分4次循环，且每次左移、右移需要移动8位。
        for (int i = 0; i < 4; i++) {
            res <<= 8;
            res |= reverseByte((byte) (n & 0b11111111));
            n >>= 8;
        }
        return res;
    }
    // 以字节为单位进行翻转，和方法一是一样的。
    public int reverseByte(byte n) {
        if (cache.containsKey(n)) {
            return cache.get(n);
        }
        int res = 0;
        // 需要一个变量temp用于循环，否则循环内会改变n的值，放入缓存会有问题。
        byte temp = n;
        for (int i = 0; i < 8; i++) {
            res <<= 1;
            res |= (temp & 1);
            temp >>= 1;
        }
        cache.put(n, res);
        return res;
    }
}
```
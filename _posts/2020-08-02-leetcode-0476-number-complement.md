---
layout: post
title: LeetCode 0476 题解
description: "数字的补数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[数字的补数](https://leetcode-cn.com/problems/number-complement/)

### 题解
1. 法一（位运算）：首先得到与给定数的位数相同且每一位都为1的数`mask`，用`mask`和给定数异或即可。
```java
class Solution {
    public int findComplement(int num) {
        int mask = 1;
        while (mask < num) {
            // 扩充位置并不断加1
            mask <<= 1;
            mask += 1;
        }
        return mask ^ num;
    }
}
```
* 另一种计算`mask`的方法，可以先得到比给定数的数位多1且只在最高位为1的数，再减1就是`mask`。
```java
class Solution {
    public int findComplement(int num) {
        int mask = 1;
        int temp = num;
        // 这里计算出的mask是比num多一位且只有最高位为1的数，所以异或时还要再减1。
        while (temp != 0) {
            mask <<= 1;
            temp >>= 1;
        }
        return (mask - 1) ^ num;
    }
}
```
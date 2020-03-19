---
layout: post
title: LeetCode 0233 题解
description: "数字 1 的个数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[数字 1 的个数](https://leetcode-cn.com/problems/number-of-digit-one/)

### 题解
1. 法一（暴力）：遍历所有数字，并对每一个数字的每一位是否等于1进行统计。（超时）
2. 法二（数学）：每一位`d`上的1可分为`d=0`、`d=1`和`d>1`进行讨论。
```java
class Solution {
    public int countDigitOne(int n) {
        int sum = 0;
        for (int k = 1; k <= n; k *= 10) {
            // xyzdabc
            int abc = n % k;
            int xyzd = n / k;
            int d = xyzd % 10;
            int xyz = xyzd / 10;
            sum += xyz * k;
            if (d == 1)
                sum += abc + 1;
            if (d > 1)
                sum += k;
            // 防止最后一次k乘上10出现溢出的情况
            if (xyz == 0)
                break;
        }
        return sum;
    }
}
```
* 改进：在`d>1`的情况，总数多加了一个`k`；因此计算`xyz`时可以先加上8，即`xyz = (xyzd + 8) / 10`，使得在`d>1`时产生进位。这样，可省略对`d>1`的讨论。同时，将`k`改为`long`类型来解决溢出问题。
```java
class Solution {
    public int countDigitOne(int n) {
        int sum = 0;
        for (long k = 1; k <= n; k *= 10) {
            // xyzdabc
            long abc = n % k;
            long xyzd = n / k;
            long d = xyzd % 10;
            long xyz = (xyzd + 8) / 10;
            sum += xyz * k;
            if (d == 1)
                sum += abc + 1;
        }
        return sum;
    }
}
```

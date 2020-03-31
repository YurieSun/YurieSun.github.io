---
layout: post
title: 剑指Offer 56 - I 题解
description: "数组中数字出现的次数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[数组中数字出现的次数](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)

### 题解
1. 法一（位运算）：先算出数组中所有元素进行异或的结果`temp`，则`temp`是两个只出现一次的数的异或结果，接下来要把这两个数分开。找到`temp`从低位起第一次出现`1`的位置`mask`，则这两个数在这一位上一定是一个为`0`，一个为`1`的情况，通过与运算就可以将它们分开。再次遍历数组，将与`mask`相与为`1`的数分为一组，为`0`的数分为另一组；则出现两次的数字一定会在同一组，进行异或运算时将会变为`0`。最终，两组数进行异或的结果就分别是这两个出现一次的数。
```java
class Solution {
    public int[] singleNumbers(int[] nums) {
        int temp = 0;
        int[] res = new int[2];
        for (int n : nums)
            temp ^= n;
        // temp从低位起第一次出现1的位置
        int mask = temp & (-temp);
        // 分组。
        for (int n : nums) {
            if ((n & mask) == 0)
                res[0] ^= n;
            else
                res[1] ^= n;
        }
        return res;
    }
}
```
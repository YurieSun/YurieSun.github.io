---
layout: post
title: LeetCode 0168 题解
description: "Excel表列名称"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[Excel表列名称](https://leetcode-cn.com/problems/excel-sheet-column-title/)

### 题解
1. 法一：相当于将一个数转换成26进制的问题，只是计数是从1开始而不是从0开始的，因此每次计算都需要减1。
```java
class Solution {
    public String convertToTitle(int n) {
        char[] map = "ABCDEFGHIJKLMNOPQRSTUVWXYZ".toCharArray();
        StringBuilder sb = new StringBuilder();
        while (n != 0) {
            n--;
            sb.append(map[n % 26]);
            n /= 26;
        }
        return sb.reverse().toString();
    }
}
```
* 递归版本
```java
class Solution {
    public String convertToTitle(int n) {
        if (n == 0)
            return "";
        n--;
        return convertToTitle(n / 26) + (char) (n % 26 + 'A');
    }
}
```
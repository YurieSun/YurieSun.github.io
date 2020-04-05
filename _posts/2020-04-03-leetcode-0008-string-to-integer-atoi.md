---
layout: post
title: LeetCode 0008 题解
description: "字符串转换整数 (atoi)"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[字符串转换整数 (atoi)](https://leetcode-cn.com/problems/string-to-integer-atoi/)

### 题解
1. 法一：直接进行判断。
```java
class Solution {
    public int myAtoi(String str) {
        if (str == null || str.length() == 0)
            return 0;
        int i = 0;
        int sign = 1;
        // 跳过前面的空格
        while (i < str.length() && str.charAt(i) == ' ') {
            i++;
        }
        // 字符串都是空格的情况
        if (i == str.length())
            return 0;
        //处理符号位
        if (str.charAt(i) == '+') {
            i++;
        } else if (str.charAt(i) == '-') {
            i++;
            sign = -1;
        }
        int res = 0;
        int max = Integer.MAX_VALUE / 10;
        while (i < str.length()) {
            // 有其它符号直接返回
            if (str.charAt(i) < '0' || str.charAt(i) > '9')
                break;
            int digit = str.charAt(i) - '0';
            // 先检查是否越界
            if (res > max || (res == max && (sign == -1 ? digit > 8 : digit > 7))) {
                return sign == -1 ? Integer.MIN_VALUE : Integer.MAX_VALUE;
            }
            res = res * 10 + digit;
            i++;
        }
        if (sign == -1)
            res *= -1;
        return res;
    }
}
```
2. 法二：有限状态机（DFA）
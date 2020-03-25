---
layout: post
title: 剑指Offer 46 题解
description: "把数字翻译成字符串"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[把数字翻译成字符串](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

### 题解
1. 法一（递归）：从后往前看，如果最后两个数字在`0`到`25`之间，就有两种方案，并往前递归；否则，只有一种方案，只能将这两个数字拆开，也往前递归。
```java
class Solution {
    public int translateNum(int num) {
        if (num <= 9)
            return 1;
        int lastTwo = num % 100;
        if (lastTwo <= 9 || lastTwo >= 26)
            return translateNum(num / 10);
        else
            return translateNum(num / 10) + translateNum(num / 100);
    }
}
```
2.法二（动态规划）：对于当前数字，都可以看成单个的，也可以考察该数字与其前一个数字结合的两位数是否满足给定范围。
```java
class Solution {
    public int translateNum(int num) {
        char[] c = String.valueOf(num).toCharArray();
        int len = c.length;
        int[] dp = new int[len + 1];
        dp[0] = 1;
        for (int i = 1; i <= len; i++) {
            // 看成单个数字
            dp[i] += dp[i - 1];
            if (i > 1) {
                // 第i-1个数字和第i个数字相结合
                int two = (c[i - 2] - '0') * 10 + (c[i - 1] - '0');
                if (two >= 10 && two <= 25)
                    dp[i] += dp[i - 2];
            }
        }
        return dp[len];
    }
}
```
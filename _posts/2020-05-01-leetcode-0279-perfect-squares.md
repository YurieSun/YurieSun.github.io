---
layout: post
title: LeetCode 0279 题解
description: "完全平方数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[完全平方数](https://leetcode-cn.com/problems/perfect-squares/)

### 题解
1. 法一（动态规划）：状态方程和斐波那契数列相同，需要注意各个情况的讨论。
```java
class Solution {
    public int numDecodings(String s) {
        if (s == null || s.length() == 0)
            return 0;
        int n = s.length();
        int[] dp = new int[n + 1];
        dp[0] = 1;
        dp[1] = s.charAt(0) == '0' ? 0 : 1;
        for (int i = 2; i <= n; i++) {
            int one = s.charAt(i - 1) - '0';
            // 当前字符不为0，才可以看成单个字符进行解码。
            if (one != 0)
                dp[i] = dp[i - 1];
            // 若当前字符的前一个字符为0，则只能将当前字符看成一个字符，无法与前一个字符组成两位数。
            if (s.charAt(i - 2) == '0')
                continue;
            int two = Integer.valueOf(s.substring(i - 2, i));
            if (two <= 26) {
                dp[i] += dp[i - 2];
            }
        }
        return dp[n];
    }
}
```
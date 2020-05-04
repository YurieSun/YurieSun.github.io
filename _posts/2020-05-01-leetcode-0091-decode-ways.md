---
layout: post
title: LeetCode 0091 题解
description: "解码方法"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[解码方法](https://leetcode-cn.com/problems/decode-ways/)

### 题解
1. 法一（动态规划）：状态方程和斐波那契数列相同，需要注意各个情况的讨论。具体如下所示（当前字符为s[i-1]）。可以简化为以下判断：若当前字符不为0，则先将dp[i]=dp[i-1]（当前字符不为0的情况与为0的情况相比，多加了dp[i-1]）；若前一个字符为0，可以直接返回（情况1.1结果为0，情况2.1结果为dp[i-1]）；若当前字符不为0，且这个两位数小于等于26，说明还可以看成两位数，继续加上dp[i-2]。
* s[i-1] == 0  
    (1.1) s[i-2] == 0：连续的00不能解码，结果为0；  
    (1.2) s[i-2] != 0  
        s[i-2]s[i-1] <= 26：若看成两位数，小于等于26可解码，则有dp[i] = dp[i-2]；  
        s[i-2]s[i-1] > 26：若看成两位数，大于26不能解码，结果为0；  
* s[i-1] != 0  
    (2.1) s[i-2] == 0：不能有以0开头的两位数，因此只有一位数一种方案，则有dp[i] = dp[i-1]；  
    (2.2) s[i-2] != 0  
        s[i-2]s[i-1] <= 26：若看成两位数，小于等于26可解码，因此有一位数和两位数两种方案，则有dp[i] = dp[i-1] + dp[i-2]；  
        s[i-2]s[i-1] > 26：若看成两位数，大于26不能解码，因此只有一位数一种方案，则有dp[i] = dp[i-1]。

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
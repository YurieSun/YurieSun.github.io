---
layout: post
title: LeetCode 0010 题解
description: "正则表达式匹配"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[正则表达式匹配](https://leetcode-cn.com/problems/regular-expression-matching/)

### 题解
1. 法一（动态规划）
```java
class Solution {
    public boolean isMatch(String s, String p) {
        int m = s.length(), n = p.length();
        boolean[][] dp = new boolean[m + 1][n + 1];
        // 初始化
        dp[0][0] = true;
        for(int i = 1; i <= n; i++){
            if(p.charAt(i-1) == '*')
                dp[0][i] = dp[0][i-2];
        }
        // 分情况讨论
        for(int i = 1; i <= m; i++)
            for(int j = 1; j <= n; j++){
                // s的第i-1个字符和p的第j-1个字符能对上或者可以替换为任意字符的情况
                if(s.charAt(i-1) == p.charAt(j-1) || p.charAt(j-1) == '.')
                    dp[i][j] = dp[i-1][j-1];
                // 对不上时，如果p的第j-1个字符是‘*’，还有机会配上
                else if(p.charAt(j-1) == '*'){
                    // 这时p的第j-2个字符加上*也无法匹配，此时让这两个字符都消失，即p[i-2]重复0次
                    if(p.charAt(j-2) != s.charAt(i-1) && p.charAt(j-2) != '.')
                        dp[i][j] = dp[i][j-2];
                    // 可以配上，让p[i-2]分别重复0次、1次、多次，任意一种配上即可
                    else
                        dp[i][j] = (dp[i][j-2] || dp[i-1][j-2] || dp[i-1][j]);
                }
            }
        return dp[m][n];
    }
}
```

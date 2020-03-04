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
                if(s.charAt(i-1) == p.charAt(j-1) || p.charAt(j-1) == '.')
                    dp[i][j] = dp[i-1][j-1];
                else if(p.charAt(j-1) == '*'){
                    if(p.charAt(j-2) != s.charAt(i-1) && p.charAt(j-2) == '.')
                        dp[i][j] = dp[i][j-2];
                    else
                        dp[i][j] = (dp[i-1][j] || dp[i][j-1] || dp[i][j-2]);
                }
            }
        return dp[m][n];
    }
}
```
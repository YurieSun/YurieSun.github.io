---
layout: post
title: LeetCode 0139 题解
description: "单词拆分"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[单词拆分](https://leetcode-cn.com/problems/word-break/)

### 题解
1. 法一（动态规划）：可将本题看成一个完全背包问题。其中，字符串`s`看成背包，字典`wordDict`看成物品，字典中每个字符的长度可看成重量，则可转换为是否存在一种挑选物品的方案使得背包装满的问题。从字典中挑选的顺序不同，组成的字符串也不同，因此是一个有顺序的背包问题，需要将物品的循环放在最内层。
```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        int W = s.length();
        boolean dp[] = new boolean[W + 1];
        dp[0] = true;
        for (int i = 1; i <= W; i++) {
            for (String word : wordDict) {
                int len = word.length();
                if (len <= i && word.equals(s.substring(i - len, i))) {
                    dp[i] = dp[i] || dp[i - len];
                }
            }
        }
        return dp[W];
    }
}
```
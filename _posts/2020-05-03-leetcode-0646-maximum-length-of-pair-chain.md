---
layout: post
title: LeetCode 0646 题解
description: "最长数对链"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[最长数对链](https://leetcode-cn.com/problems/maximum-length-of-pair-chain/)

### 题解
1. 法一（动态规划）：先根据数组中第一个数进行排序，再用LIS的方法即可。
```java
class Solution {
    class Solution {
    public int findLongestChain(int[][] pairs) {
        int[] dp = new int[pairs.length];
        Arrays.fill(dp, 1);
        // 通过lamda表达式传入Comparator
        Arrays.sort(pairs, (a, b) -> a[0] - b[0]);
        for (int i = 1; i < pairs.length; i++) {
            for (int j = 0; j < i; j++) {
                if (pairs[i][0] > pairs[j][1]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
        }
        int res = 0;
        for (int x : dp) {
            res = Math.max(res, x);
        }
        return res;
    }
}
```
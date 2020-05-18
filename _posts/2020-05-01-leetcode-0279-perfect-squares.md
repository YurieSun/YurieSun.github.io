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
1. 法一（动态规划）
```java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            // 先将每个值设为最大
            dp[i] = i;
            for (int j = 1; j * j <= i; j++) {
                dp[i] = Math.min(dp[i], dp[i - j * j] + 1);
            }
        }
        return dp[n];
    }
}
```
2. 法二（BFS）：可以想成一棵以`n`为起点的多叉树，不断往下分解，直到叶子节点出现`0`，说明已经分解完，且是最少的分解个数，相当于在求最短路径。
```java
class Solution {
    public int numSquares(int n) {
        Queue<Integer> q = new LinkedList<>();
        Set<Integer> visited = new HashSet<>();
        q.add(n);
        visited.add(n);
        int step = 0;
        while (!q.isEmpty()) {
            int size = q.size();
            step++;
            for (int i = 0; i < size; i++) {
                int cur = q.poll();
                for (int j = 1; j * j <= cur; j++) {
                    int x = cur - j * j;
                    if (x == 0)
                        return step;
                    if (visited.contains(x))
                        continue;
                    q.add(x);
                    visited.add(x);
                }
            }
        }
        return -1;
    }
}
```
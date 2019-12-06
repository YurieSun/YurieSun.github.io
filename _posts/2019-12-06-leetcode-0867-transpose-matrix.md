---
layout: post
title: LeetCode 0867 题解
description: "转置矩阵"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[转置矩阵](https://leetcode-cn.com/problems/transpose-matrix/)

### 题解
1. 法一：遍历矩阵，转置元素。
```java
class Solution {
    public int[][] transpose(int[][] A) {
        int m = A.length, n = A[0].length;
        int[][] ans = new int[n][m];
        for(int i = 0; i < n; i++)
            for(int j = 0; j < m; j++)
                ans[i][j] = A[j][i];
        return ans;
    }
}
```
* 也可写为：
```java
class Solution {
    public int[][] transpose(int[][] A) {
        int m = A.length, n = A[0].length;
        int[][] ans = new int[n][m];
        for(int i = 0; i < m*n; i++)
            ans[i%n][i/n] = A[i/n][i%n];
        return ans;
    }
}
```
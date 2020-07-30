---
layout: post
title: LeetCode 0766 题解
description: "托普利茨矩阵"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[托普利茨矩阵](https://leetcode-cn.com/problems/toeplitz-matrix/)

### 题解
1. 法一：遍历矩阵，检查每个元素是否与其左上角元素相等。
```java
class Solution {
    public boolean isToeplitzMatrix(int[][] matrix) {
        for (int i = 1; i < matrix.length; i++) {
            for (int j = 1; j < matrix[0].length; j++) {
                if (matrix[i][j] != matrix[i - 1][j - 1]) {
                    return false;
                }
            }
        }
        return true;
    }
}
```
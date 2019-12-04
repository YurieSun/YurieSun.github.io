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
1. 法一：遍历矩阵，每一个元素与其左上角元素比较，只要有不相等的就返回`false`，若均满足，则返回`true`。
```java
class Solution {
    public boolean isToeplitzMatrix(int[][] matrix) {
        for(int i = 1; i < matrix.length; i++)
            for(int j = 1; j < matrix[0].length; j++) 
             if(matrix[i-1][j-1] != matrix[i][j])
                return false;
        return true;
    }
}
```
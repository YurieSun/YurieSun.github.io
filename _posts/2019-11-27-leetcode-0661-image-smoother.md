---
layout: post
title: LeetCode 0661 题解
description: "图片平滑器"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[图片平滑器](https://leetcode-cn.com/problems/image-smoother/)

### 题解
1. 法一：扫描矩阵，计算每个元素周围的和，同时计数，将每个元素更新为平均值。
```java
class Solution {
    public int[][] imageSmoother(int[][] M) {
        int m = M.length, n = M[0].length;
        int[][] ans = new int[m][n];
        for(int i = 0; i < m; i++)
            for(int j = 0; j < n; j++){
                int count = 0;
                for(int r = i-1; r <= i+1; r++)
                    for(int c = j-1; c <= j+1; c++){
                        if(r >= 0 && r < m && c >= 0 && c < n){
                            ans[i][j] += M[r][c];
                            count++;
                        }
                    }
                ans[i][j] /= count;
            }
        return ans;
    }
}
```
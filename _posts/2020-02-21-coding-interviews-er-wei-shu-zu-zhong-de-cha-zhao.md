---
layout: post
title: 剑指Offer 04 题解
description: "二维数组中的查找"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[二维数组中的查找](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

### 题解
1. 法一（暴力）：遍历矩阵查找。
```java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        if(matrix == null || matrix.length == 0 || matrix[0].length == 0)
            return false;
        int n = matrix.length, m = matrix[0].length;
        for(int i = 0; i < n; i++)
            for(int j = 0; j < m; j++){
                if(target == matrix[i][j])
                    return true;
            }
        return false;
    }
}
```
2. 法二：以上方法没有用到矩阵中数字大小的特性。如果从数组左上角开始查找，则其右边的元素和下面的元素都比它大，无法决定下一步该去哪，从右下角出发同理。因此，可以选择从右上角出发，则其左边的元素比它小，下面的元素比它大，就可以根据元素大小，决定下一步该往左走还是往下走。同理，也可以从左下角出发。
```java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        if(matrix == null || matrix.length == 0 || matrix[0].length == 0)
            return false;
        int n = matrix.length, m = matrix[0].length;
        int i = 0, j = m - 1;
        while(i < n && j >= 0){
            if(target == matrix[i][j])
                return true;
            else if(target > matrix[i][j])
                i++;
            else
                j--;
        }
        return false;
    }
}
```
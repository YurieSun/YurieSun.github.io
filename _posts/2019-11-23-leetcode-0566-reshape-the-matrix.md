---
layout: post
title: LeetCode 0566 题解
description: "重塑数组"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[重塑数组](https://leetcode-cn.com/problems/reshape-the-matrix/)

### 题解
1. 法一：将矩阵元素看成一维排列的数组，再将其按照新的行与列放入新的矩阵中。
```java
class Solution {
    public int[][] matrixReshape(int[][] nums, int r, int c) {
        int m = nums.length, n = nums[0].length;
        if(nums.length == 0 || m*n != r*c) 
            return nums;
        int[][] ans = new int[r][c];
        for(int i = 0; i < m; i++)
            for(int j = 0; j < n; j++)
                ans[(i*n+j)/c][(i*n+j)%c] = nums[i][j];
        return ans;
    }
}
```
* 以上也可写成：
```java
class Solution {
    public int[][] matrixReshape(int[][] nums, int r, int c) {
        int m = nums.length, n = nums[0].length;
        if(nums.length == 0 || m*n != r*c) 
            return nums;
        int[][] ans = new int[r][c];
        int count = 0;
        for(int i = 0; i < m; i++)
            for(int j = 0; j < n; j++){
                ans[count/c][count%c] = nums[i][j];
                count++;
            }
        return ans;
    }
}
```
* 也可写成一重循环，实质上与二重循环一样，都是对数组元素进行了一次遍历。
```java
class Solution {
    public int[][] matrixReshape(int[][] nums, int r, int c) {
        int m = nums.length, n = nums[0].length;
        if(nums.length == 0 || m*n != r*c) 
            return nums;
        int[][] ans = new int[r][c];
        for(int i = 0; i < m*n; i++)
            ans[i/c][i%c] = nums[i/n][i%n];
        return ans;
    }
}
```
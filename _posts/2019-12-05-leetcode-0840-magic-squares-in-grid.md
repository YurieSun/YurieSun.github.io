---
layout: post
title: LeetCode 0840 题解
description: "矩阵中的幻方"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[矩阵中的幻方](https://leetcode-cn.com/problems/magic-squares-in-grid/)

### 题解
1. 法一：暴力法，逐个$3\times 3$方阵进行判断。
```java
class Solution {
    public int numMagicSquaresInside(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        if(m < 3 || n < 3) return 0;
        int sum = 0;
        for(int i = 0; i < m-2; i++)
            for(int j = 0; j < n-2; j++){
                if(grid[i+1][j+1] != 5)
                    continue;
                if(isMagic(grid[i][j], grid[i][j+1], grid[i][j+2], 
                           grid[i+1][j], grid[i+1][j+1], grid[i+1][j+2], 
                           grid[i+2][j], grid[i+2][j+1], grid[i+2][j+2]))
                    sum++;
            }
        return sum;          
    }
    public boolean isMagic(int... num){
        int[] count = new int[16];
        for(int x : num)
            count[x]++;
        for(int i = 1; i <= 9; i++)
            if(count[i] != 1)
                return false;
        return num[0] + num[1] + num[2] == 15 &&
               num[3] + num[4] + num[5] == 15 &&
               num[6] + num[7] + num[8] == 15 &&
               num[0] + num[3] + num[6] == 15 &&
               num[1] + num[4] + num[7] == 15 &&
               num[2] + num[5] + num[8] == 15 &&
               num[0] + num[4] + num[8] == 15 &&
               num[2] + num[4] + num[6] == 15; 
    }
}
```
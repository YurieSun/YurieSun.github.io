---
layout: post
title: LeetCode 0200 题解
description: "岛屿数量"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

### 题解
1. 法一（DFS）
```java
class Solution {
    private int m, n;
    private int[][] dir;
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0)
            return 0;
        m = grid.length;
        n = grid[0].length;
        int res = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] != '0') {
                    dfs(grid, i, j);
                    res++;
                }
            }
        }
        return res;
    }
    private void dfs(char[][] grid, int i, int j) {
        if (i < 0 || i >= m || j < 0 || j >= n || grid[i][j] == '0')
            return;
        grid[i][j] = '0';
        for (int[] d : dir) {
            dfs(grid, i + d[0], j + d[1]);
        }
    }
}
```
* 这里的二维数组定义如下图所示（分别对应一个格子周围四个方向）：
![二维数组定义]({{ '/assets/images/LeetCode/0200.PNG' }})
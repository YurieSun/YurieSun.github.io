---
layout: post
title: LeetCode 0695 题解
description: "岛屿的最大面积"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[岛屿的最大面积](https://leetcode-cn.com/problems/max-area-of-island/)

### 题解
1. 法一（DFS）
```java
class Solution {
    private int m, n;
    private int[][] dir;
    public int maxAreaOfIsland(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0)
            return 0;
        m = grid.length;
        n = grid[0].length;
        int res = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                res = Math.max(res, dfs(grid, i, j));
            }
        }
        return res;
    }
    private int dfs(int[][] grid, int i, int j) {
        if (i < 0 || i >= m || j < 0 || j >= n || grid[i][j] == 0)
            return 0;
        grid[i][j] = 0;
        int area = 1;
        for (int[] d : dir) {
            area += dfs(grid, i + d[0], j + d[1]);
        }
        return area;
    }
}
```
* 这里的二维数组定义如下图所示（分别对应一个格子周围四个方向）：
![二维数组定义]({{ '/assets/images/LeetCode/0695.PNG' }})
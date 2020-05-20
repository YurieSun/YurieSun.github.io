---
layout: post
title: LeetCode 0417 题解
description: "太平洋大西洋水流问题"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[太平洋大西洋水流问题](https://leetcode-cn.com/problems/pacific-atlantic-water-flow/)

### 题解
1. 法一（DFS）:四条边上的点肯定能到达太平洋或大西洋，因此从四条边上的开始搜索，找到内部可以到达的点。最后再遍历数组，遭到能同时到达太平洋和大西洋的点。
```java
class Solution {
    private int m, n;
    private int[][] dir;
    public List<List<Integer>> pacificAtlantic(int[][] matrix) {
        List<List<Integer>> res = new ArrayList<>();
        if (matrix == null || matrix.length == 0 || matrix[0] == null || matrix[0].length == 0)
            return res;
        m = matrix.length;
        n = matrix[0].length;
        boolean[][] canReachP = new boolean[m][n];
        boolean[][] canReachA = new boolean[m][n];
        for (int i = 0; i < m; i++) {
            // 以左边界（太平洋）为起点可以到达的点
            dfs(matrix, i, 0, canReachP);
            // 以右边界（大西洋）为起点可以到达的点
            dfs(matrix, i, n - 1, canReachA);
        }
        for (int i = 0; i < n; i++) {
            // 以上边界（太平洋）为起点可以到达的点
            dfs(matrix, 0, i, canReachP);
            // 以下边界（大西洋）为起点可以到达的点
            dfs(matrix, m - 1, i, canReachA);
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (canReachP[i][j] && canReachA[i][j]) {
                    res.add(Arrays.asList(i, j));
                }
            }
        }
        return res;
    }
    private void dfs(int[][] matrix, int i, int j, boolean[][] canReach) {
        if (canReach[i][j])
            return;
        canReach[i][j] = true;
        for (int[] d : dir) {
            int newX = i + d[0];
            int newY = j + d[1];
            if (newX >= 0 && newX < m && newY >= 0 && newY < n && matrix[newX][newY] >= matrix[i][j]) {
                dfs(matrix, newX, newY, canReach);
            }
        }
    }
}
```
* 这里的二维数组定义如下图所示（分别对应一个格子周围四个方向）：
![二维数组定义]({{ '/assets/images/LeetCode/0417.PNG' }})
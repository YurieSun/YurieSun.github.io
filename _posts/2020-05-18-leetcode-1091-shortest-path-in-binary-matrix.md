---
layout: post
title: LeetCode 1091 题解
description: "二进制矩阵中的最短路径"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[二进制矩阵中的最短路径](https://leetcode-cn.com/problems/shortest-path-in-binary-matrix/)

### 题解
1. 法一（BFS）：需要注意的是，何时将访问的点打上标记，对于已经打上标记的点何时进行判断。
```java
class Solution {
    public int shortestPathBinaryMatrix(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0)
            return -1;
        int m = grid.length, n = grid[0].length;
        if (grid[0][0] == 1 || grid[m - 1][n - 1] == 1)
            return -1;
        Queue<int[]> q = new LinkedList<>();
        int[][] dir;
        q.add(new int[]{0, 0});
        grid[0][0] = 1;
        int step = 1;
        while (!q.isEmpty()) {
            int size = q.size();
            for (int i = 0; i < size; i++) {
                int[] cur = q.poll();
                if (cur[0] == m - 1 && cur[1] == n - 1)
                    return step;
                for (int[] d : dir) {
                    int newX = cur[0] + d[0];
                    int newY = cur[1] + d[1];
                    if (newX >= 0 && newX < m && newY >= 0 && newY < n && grid[newX][newY] == 0) {
                        q.add(new int[]{newX, newY});
                        grid[newX][newY] = 1;
                    }
                }
            }
            step++;
        }
        return -1;
    }
}
```
* 这里的二维数组定义如下图所示（分别对应一个格子周围八个方向）：
![二维数组定义]({{ '/assets/images/LeetCode/1091.PNG' }})
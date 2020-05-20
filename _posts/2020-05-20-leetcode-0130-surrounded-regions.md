---
layout: post
title: LeetCode 0130 题解
description: "被围绕的区域"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[被围绕的区域](https://leetcode-cn.com/problems/surrounded-regions/)

### 题解
1. 法一（DFS）:从四条边开始，将能与边界上的'O'相连的'O'打上标记，随后遍历数组，打上标记的'O'重新设为'O'，未打上标记的'O'改为'X'。
```java
class Solution {
    private int m, n;
    private int[][] dir;
    public void solve(char[][] board) {
        if (board == null || board.length == 0 || board[0].length == 0)
            return;
        m = board.length;
        n = board[0].length;
        for (int i = 0; i < m; i++) {
            dfs(board, i, 0);
            dfs(board, i, n - 1);
        }
        for (int i = 0; i < n; i++) {
            dfs(board, 0, i);
            dfs(board, m - 1, i);
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 'T')
                    board[i][j] = 'O';
                else if (board[i][j] == 'O')
                    board[i][j] = 'X';
            }
        }

    }
    private void dfs(char[][] board, int i, int j) {
        if (i < 0 || i >= m || j < 0 || j >= n || board[i][j] != 'O')
            return;
        board[i][j] = 'T';
        for (int[] d : dir) {
            dfs(board, i + d[0], j + d[1]);
        }
    }
}
```
2. 法二（BFS）
```java
class Solution {
    private int m, n;
    private int[][] dir;
    public void solve(char[][] board) {
        if (board == null || board.length == 0 || board[0].length == 0)
            return;
        m = board.length;
        n = board[0].length;
        for (int i = 0; i < m; i++) {
            bfs(board, i, 0);
            bfs(board, i, n - 1);
        }
        for (int i = 0; i < n; i++) {
            bfs(board, 0, i);
            bfs(board, m - 1, i);
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 'T')
                    board[i][j] = 'O';
                else if (board[i][j] == 'O')
                    board[i][j] = 'X';
            }
        }

    }
    private void bfs(char[][] board, int i, int j) {
        Queue<int[]> q = new LinkedList<>();
        if (board[i][j] == 'O') {
            q.add(new int[]{i, j});
            board[i][j] = 'T';
        }
        while (!q.isEmpty()) {
            int[] cur = q.poll();
            for (int[] d : dir) {
                int newX = cur[0] + d[0];
                int newY = cur[1] + d[1];
                if (newX >= 0 && newX < m && newY >= 0 && newY < n && board[newX][newY] == 'O') {
                    q.add(new int[]{newX, newY});
                    board[newX][newY] = 'T';
                }
            }
        }
    }
}
```
* 这里的二维数组定义如下图所示（分别对应一个格子周围四个方向）：
![二维数组定义]({{ '/assets/images/LeetCode/0130.PNG' }})
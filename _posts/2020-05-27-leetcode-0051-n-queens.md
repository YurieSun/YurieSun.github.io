---
layout: post
title: LeetCode 0051 题解
description: "N皇后"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[N皇后](https://leetcode-cn.com/problems/n-queens/)

### 题解
1. 法一（回溯）
```java
class Solution {
    private List<List<String>> res = new ArrayList<>();
    private int size;
    public List<List<String>> solveNQueens(int n) {
        if (n <= 0)
            return res;
        char[][] board = new char[n][n];
        size = n;
        // 构造棋盘
        for (char[] c : board) {
            Arrays.fill(c, '.');
        }
        backtrace(board, 0);
        return res;
    }
    // 回溯函数
    private void backtrace(char[][] board, int row) {
        if (row == board.length) {
            addSolution(board);
            return;
        }
        for (int col = 0; col < size; col++) {
            if (!isValid(board, row, col))
                continue;
            board[row][col] = 'Q';
            // 一行只能放一个，因此在当前行放完后直接走下一行。
            backtrace(board, row + 1);
            board[row][col] = '.';
        }
    }
    // 检验当前位置能不能放
    private boolean isValid(char[][] board, int row, int col) {
        // 检查同一列
        for (int i = 0; i < size; i++) {
            if (board[i][col] == 'Q')
                return false;
        }
        // 检查右上角
        for (int i = row - 1, j = col + 1; i >= 0 && j < size; i--, j++) {
            if (board[i][j] == 'Q')
                return false;
        }
        // 检查左上角
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (board[i][j] == 'Q')
                return false;
        }
        return true;
    }
    // 将可行解放入结果集
    private void addSolution(char[][] board) {
        List<String> path = new ArrayList<>();
        for (int i = 0; i < size; i++) {
            StringBuilder sb = new StringBuilder();
            for (int j = 0; j < size; j++) {
                sb.append(board[i][j]);
            }
            path.add(sb.toString());
        }
        res.add(path);
    }
}
```
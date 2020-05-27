---
layout: post
title: LeetCode 0037 题解
description: "解数独"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[解数独](https://leetcode-cn.com/problems/sudoku-solver/)

### 题解
1. 法一（回溯）
```java
class Solution {
    public void solveSudoku(char[][] board) {
        backtrace(board, 0, 0);
    }
    // 回溯函数
    private boolean backtrace(char[][] board, int i, int j) {
        int m = 9, n = 9;
        // 若走到每一行的最后一个格子，就从下一行第一个格子开始。
        if (j == n) {
            return backtrace(board, i + 1, 0);
        }
        // 若走完最后一行，说明找到了解，可以返回。
        if (i == m) {
            return true;
        }
        // 若当前格子上是初始给定的数字，则跳过当前格子，走下一个格子。
        if (board[i][j] != '.') {
            return backtrace(board, i, j + 1);
        }
        // 需要穷举的格子
        for (char ch = '1'; ch <= '9'; ch++) {
            if (!isValid(board, i, j, ch))
                continue;
            board[i][j] = ch;
            // 找到一种方案则提前返回。
            if (backtrace(board, i, j + 1))
                return true;
            board[i][j] = '.';
        }
        return false;
    }
    // 判断一个数是否出现在所在行、所在列、所在3x3方框。
    private boolean isValid(char[][] board, int r, int c, char ch) {
        for (int i = 0; i < 9; i++) {
            if (board[r][i] == ch)
                return false;
            if (board[i][c] == ch)
                return false;
            // 这里遍历3x3方框的方法很巧妙。可以这样理解：
            // 将9x9矩阵看成9个3x3方框，方框的坐标分别为[0,0], [0,1], ..., [2,1],[2,2]，即[r/3][c/3]。
            // [(r/3)*3][(c/3)*3] 是每个数字所在方框左上角的坐标。
            // 横、纵坐标分别加上 i/3、i%3 就是从左上角的格子开始遍历，每3个格子则走到下一行，就可以将方框内9个格子遍历完。
            if (board[(r / 3) * 3 + i / 3][(c / 3) * 3 + i % 3] == ch)
                return false;
        }
        return true;
    }
}
```
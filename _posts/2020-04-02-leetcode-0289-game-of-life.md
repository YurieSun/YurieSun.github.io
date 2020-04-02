---
layout: post
title: LeetCode 0289 题解
description: "生命游戏"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[生命游戏](https://leetcode-cn.com/problems/game-of-life/)

### 题解
1. 法一：首先根据细胞更新后状态和更新前状态定义四种状态，分别为：`00`（更新后死，更新前死）、`01`（更新后死，更新前活）、`10`（更新后活，更新前死）、`11`（更新后活，更新前活）；其十进制所表示的数分别为`0`、`1`、`2`、`3`。因此，对于每个细胞，首先遍历其周围8个格子，得到周围活细胞的个数。对于`00`和`01`，原矩阵中对应的值就是`0`和`1`，用这种状态表示的值仍为`0`和`1`，因此不需要进行更新。对于`10`和`11`，当满足后活前死，则将该细胞位置上的数更新为`2`；当满足后活前活，则将该细胞位置上的数更新为`3`。这样做可以区分这四种状态，且更新完整个矩阵后，再次进行遍历，并将新数右移一位，就可以得到更新后状态。
```java
class Solution {
    private int[][] board;
    private int m;
    private int n;
    private int[] dx = new int[]{-1, -1, -1, 0, 0, 1, 1, 1};
    private int[] dy = new int[]{-1, 0, 1, -1, 1, -1, 0, 1};
    public void gameOfLife(int[][] board) {
        if (board == null || board[0] == null || board.length == 0 || board[0].length == 0)
            return;
        this.board = board;
        m = board.length;
        n = board[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // 得到周围8个格子中活细胞的个数
                int cnt = countAlive(i, j);
                // 满足后活前死，更新为2
                if (board[i][j] == 0 && cnt == 3)
                    board[i][j] = 2;
                // 满足后活前活，更新为3
                if (board[i][j] == 1 && (cnt == 2 || cnt == 3))
                    board[i][j] = 3;
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // 右移一位，舍去更新前状态，得到更新后状态。
                board[i][j] >>= 1;
            }
        }
    }
    private int countAlive(int i, int j) {
        int cnt = 0;
        for (int k = 0; k < 8; k++) {
            int x = i + dx[k];
            int y = j + dy[k];
            if (x < 0 || x >= m || y < 0 || y >= n)
                continue;
            // 计数时要注意，由于之前的数字已被更新过，不可以直接用0、1判断死活。
            // 而是要通过新的四种状态获得，即新状态中的后一位数，和1相与即得。
            cnt += (board[x][y] & 1);
        }
        return cnt;
    }
}
```
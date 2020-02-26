---
layout: post
title: LeetCode 0079 题解
description: "单词搜索"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[单词搜索](https://leetcode-cn.com/problems/word-search/)

### 题解
1. 法一（dfs）：对于每一个匹配的字符，对其上下左右进行搜索，查看是否有下一个字符，有则继续往下搜索，没有则回溯。
```java
class Solution {
    private int m, n;
    private String word;
    private char[][] board;
    private boolean marked[][];
    private int[][] directions;

    public boolean exist(char[][] board, String word) {
        m = board.length;
        if(m == 0)
            return false;
        n = board[0].length;
        this.word = word;
        this.board = board;
        marked = new boolean[m][n];
        directions = new int[][]{{-1, 0}, {0, -1}, {1, 0}, {0, 1}};
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++)
                if(dfs(i, j, 0))
                    return true;
        }
        return false;

    }
    private boolean dfs(int i, int j, int start){
        if(start == word.length() - 1)
            return board[i][j] == word.charAt(start);
        if(board[i][j] == word.charAt(start)){
            marked[i][j] = true;
            for(int k = 0; k < 4; k++){
                int newX = i + directions[k][0];
                int newY = j + directions[k][1];
                if(inArea(newX, newY) && !marked[newX][newY]){
                    if(dfs(newX, newY, start + 1))
                        return true;
                }
            }
            //这一步不能省略，这里表示当前位置的上下左右都找不到下一个字符，那么当前位置不应该做为路径中的一个点；
            //因此应该要重置为false，这是回溯的过程。
            marked[i][j] = false;
        }
        return false;
    }
    private boolean inArea(int x, int y){
        return x >= 0 && x < m && y >= 0 && y < n;
    }
}
```
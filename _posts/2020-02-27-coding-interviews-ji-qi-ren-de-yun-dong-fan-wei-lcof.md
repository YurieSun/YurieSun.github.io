---
layout: post
title: 剑指Offer 13 题解
description: "机器人的运动范围"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)

### 题解
1. 法一（DFS）：满足数位和条件的数组构成多个等腰直角三角形，若这些三角形不连通，只能计算左上方三角形的格子数；若连通时，则可以计算连通三角形的格子数。根据这种结构，则往右或往下走即可走遍所有满足条件的格子。因此，采用`dfs`时，若格子越界、已经访问、数位和不满足条件，则可直接返回；否则，计算当前格子数`1`，并加上往右和往下走的格子数（继续递归）。
```java
class Solution {
    private int m, n, k;
    private boolean [][] marked;
    public int movingCount(int m, int n, int k) {
        if(m == 0)
            return 0;
        this.m = m;
        this.n = n;
        this.k = k;
        marked = new boolean[m][n];
        return dfs(0,0);
    }
    private int dfs(int i, int j){
        if((i < 0 || i >= m || j < 0 || j >= n) || marked[i][j] || digitSum(i) + digitSum(j) > k)
            return 0;
        marked[i][j] = true;
        return 1 + dfs(i, j + 1) + dfs(i + 1, j);
    }
    private int digitSum(int num){
        int sum = 0;
        while(num != 0){
            sum += num % 10;
            num /= 10;
        }
        return sum;
    }
}
```
* 改进：由于每次只走一格，因此计算数位和不需要每次都进行函数的计算。将`x`的数位和记为`s`，则当`(x+1)%10=0`时，`x+1`的数位和为`s-8`，反之为`s+1`。
```java
class Solution {
    private int m, n, k;
    private boolean [][] marked;
    public int movingCount(int m, int n, int k) {
        if(m == 0)
            return 0;
        this.m = m;
        this.n = n;
        this.k = k;
        marked = new boolean[m][n];
        return dfs(0, 0, 0, 0);
    }
    private int dfs(int i, int j, int si, int sj){
        if((i < 0 || i >= m || j < 0 || j >= n) || marked[i][j] || si + sj > k)
            return 0;
        marked[i][j] = true;
        return 1 + dfs(i, j + 1, si, ((j + 1) % 10 == 0) ? sj - 8: sj + 1) 
                 + dfs(i + 1, j, ((i + 1) % 10 == 0) ? si - 8: si + 1, sj);
    }
}
```
2. 法二（BFS）：和树的层次遍历差不多，先将左上方第一个格子入队，再分别将其下方和右方的格子入队，进行迭代，并更新满足条件的格子数。
```java
class Solution {
    private int m, n, k;
    private boolean [][] marked;
    public int movingCount(int m, int n, int k) {
        if(m == 0)
            return 0;
        this.m = m;
        this.n = n;
        this.k = k;
        int cnt = 0;
        marked = new boolean[m][n];
        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[] {0, 0});
        while(!queue.isEmpty()){
            int[] cur = queue.poll();
            int i = cur[0];
            int j = cur[1];
            if((i < 0 || i >= m || j < 0 || j >= n) || marked[i][j] || digitSum(i) + digitSum(j) > k)
                continue;
            cnt++;
            marked[i][j] = true;
            queue.add(new int[] {i, j + 1});
            queue.add(new int[] {i + 1, j});
        }
        return cnt;
    }
    private int digitSum(int num){
        int sum = 0;
        while(num != 0){
            sum += num % 10;
            num /= 10;
        }
        return sum;
    }
}
```
* 改进：同样是改进数位和的计算方法，将格子的行、列、数位和都进行入队。
```java
class Solution {
    private int m, n, k;
    private boolean [][] marked;
    public int movingCount(int m, int n, int k) {
        if(m == 0)
            return 0;
        this.m = m;
        this.n = n;
        this.k = k;
        int cnt = 0;
        marked = new boolean[m][n];
        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[] {0, 0, 0, 0});
        while(!queue.isEmpty()){
            int[] cur = queue.poll();
            int i = cur[0];
            int j = cur[1];
            int si = cur[2];
            int sj = cur[3];
            if((i < 0 || i >= m || j < 0 || j >= n) || marked[i][j] || si + sj > k)
                continue;
            cnt++;
            marked[i][j] = true;
            queue.add(new int[] {i, j + 1, si, (j + 1) % 10 == 0 ? sj - 8 : sj + 1});
            queue.add(new int[] {i + 1, j, (i + 1) % 10 == 0 ? si - 8 : si + 1, sj});
        }
        return cnt;
    }
}
```
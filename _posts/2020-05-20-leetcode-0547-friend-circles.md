---
layout: post
title: LeetCode 0547 题解
description: "朋友圈"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[朋友圈](https://leetcode-cn.com/problems/friend-circles/)

### 题解
1. 法一（并查集）
```java
class Solution {
    public int findCircleNum(int[][] M) {
        if (M == null || M.length == 0 || M[0].length == 0)
            return 0;
        int m = M.length;
        int n = M[0].length;
        int res = 0;
        UF uf = new UF(m);
        // 由于矩阵是对称矩阵，且对角线上一定是1，因此只需遍历矩阵的一半即可。
        for (int i = 0; i < m; i++) {
            for (int j = i + 1; j < n; j++) {
                if (M[i][j] == 1) {
                    uf.union(i, j);
                }
            }
        }
        return uf.getCount();
    }

    class UF {
        // parent[i]表示i的父节点，根节点是parent[i]=i的节点
        // size[i]表示以i为根的节点个数
        private int count;
        private int[] parent;
        private int[] size;
        UF(int n) {
            count = n;
            parent = new int[n];
            size = new int[n];
            // 初始化时，将所有节点的根设为自己，以每个节点为根的节点个数为1，相当于每个点都是一个单独的集合。
            for (int i = 0; i < n; i++) {
                parent[i] = i;
                size[i] = 1;
            }
        }
        // 找到节点的根
        public int find(int x) {
            while (parent[x] != x) {
                // 路径压缩：找根节点的过程中，不断将自己挂到父节点的父节点上。
                parent[x] = parent[parent[x]];
                x = parent[x];
            }
            return x;
        }
        // 查询两点是否在同一个集合中
        public boolean isConnected(int p, int q) {
            int rootp = find(p);
            int rootq = find(q);
            return rootp == rootq;
        }
        // 连接两点
        public void union(int p, int q) {
            int rootp = find(p);
            int rootq = find(q);
            if (rootp == rootq)
                return;
            // 保持平衡：通过记录树的大小，将小树挂在大树下，减小树的高度。
            if (size[rootp] > size[rootq]) {
                parent[rootq] = rootp;
                size[rootp] += size[rootq];
            } else {
                parent[rootp] = rootq;
                size[rootq] += size[rootp];
            }
            count--;
        }
        // 返回并查集个数
        public int getCount() {
            return count;
        }
    }
}
```
2. 法二（DFS）
```java
class Solution {
    private int n;
    private boolean[] visited;
    public int findCircleNum(int[][] M) {
        if (M == null || M.length == 0 || M[0].length == 0)
            return 0;
        n = M.length;
        visited = new boolean[n];
        int res = 0;
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                dfs(M, i);
                res++;
            }
        }
        return res;
    }
    private void dfs(int[][] M, int i) {
        visited[i] = true;
        for (int j = 0; j < n; j++) {
            if (M[i][j] == 1 && !visited[j]) {
                dfs(M, j);
            }
        }
    }
}
```
3. 法三（BFS）
```java
class Solution {
    private int n;
    private boolean[] visited;
    public int findCircleNum(int[][] M) {
        if (M == null || M.length == 0 || M[0].length == 0)
            return 0;
        n = M.length;
        visited = new boolean[n];
        int res = 0;
        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                q.add(i);
                while (!q.isEmpty()) {
                    int cur = q.poll();
                    visited[cur] = true;
                    for (int j = 0; j < n; j++) {
                        if (M[cur][j] == 1 && !visited[j])
                            q.add(j);
                    }
                }
                res++;
            }
        }
        return res;
    }
}
```
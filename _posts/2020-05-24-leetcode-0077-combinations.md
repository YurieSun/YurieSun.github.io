---
layout: post
title: LeetCode 0077 题解
description: "组合"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[组合](https://leetcode-cn.com/problems/combinations/)

### 题解
1. 法一（回溯）
```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> combine(int n, int k) {
        if (n < 1 || k < 1 || k > n)
            return res;
        backtrace(n, k, 1, new ArrayList<Integer>());
        return res;
    }
    private void backtrace(int n, int k, int start, ArrayList<Integer> path) {
        if (path.size() == k) {
            res.add(new ArrayList<>(path));
            return;
        }
        for (int i = start; i <= n; i++) {
            path.add(i);
            backtrace(n, k, i + 1, path);
            path.remove(path.size() - 1);
        }
    }
}
```
* 改进：进行剪枝。
```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> combine(int n, int k) {
        if (n < 1 || k < 1 || k > n)
            return res;
        backtrace(n, k, 1, new ArrayList<Integer>());
        return res;
    }
    private void backtrace(int n, int k, int start, ArrayList<Integer> path) {
        if (path.size() == k) {
            res.add(new ArrayList<>(path));
            return;
        }
        // 主要是通过控制i的上界进行剪枝；
        // k - path.size() 表示还需要选择的元素个数，而n - (k - path.size()) + 1 表示从n往前数这么多个元素的起始位置，
        // 超过这个位置后，就不够选择这么多元素，因此这就是i的上界。
        for (int i = start; i <= n - (k - path.size()) + 1; i++) {
            path.add(i);
            backtrace(n, k, i + 1, path);
            path.remove(path.size() - 1);
        }
    }
}
```
---
layout: post
title: LeetCode 0257 题解
description: "二叉树的所有路径"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[二叉树的所有路径](https://leetcode-cn.com/problems/binary-tree-paths/)

### 题解
1. 法一（回溯）
```java
class Solution {
    List<String> res = new ArrayList<>();
    public List<String> binaryTreePaths(TreeNode root) {
        if (root == null)
            return res;
        backtrace(root, new ArrayList());
        return res;
    }
    private void backtrace(TreeNode root, ArrayList<Integer> path) {
        // 节点为空则返回
        if (root == null) {
            return;
        }
        path.add(root.val);
        // 叶子节点则将路径加入
        if (root.left == null && root.right == null){
            res.add(buildPath(path));
        }
        // 非叶子结点则继续回溯
        else {
            backtrace(root.left, path);
            backtrace(root.right, path);
        }
        path.remove(path.size() - 1);
    }
    // 根据路径上的节点值建立对应字符串表示
    private String buildPath(ArrayList<Integer> path) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < path.size() - 1; i++) {
            sb.append(path.get(i)).append("->");
        }
        sb.append(path.get(path.size() - 1));
        return sb.toString();
    }
}
```
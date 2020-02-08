---
layout: post
title: LeetCode 0669 题解
description: "修剪二叉搜索树"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[修剪二叉搜索树](https://leetcode-cn.com/problems/trim-a-binary-search-tree/comments/)

### 题解
1. 法一（递归）
```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        dfs(root, res);
        return res;
    }
    private void dfs(TreeNode root, List<Integer> list){
        if(root != null){
            dfs(root.left, list);
            list.add(root.val);
            dfs(root.right, list);
        }
    }
}
```
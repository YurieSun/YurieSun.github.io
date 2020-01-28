---
layout: post
title: LeetCode 0226 题解
description: "翻转二叉树"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)

### 题解
1. 法一（递归）：DFS
* 前序遍历
```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null)
            return null;
        TreeNode left = root.left;
        root.left = root.right;
        root.right = left;
        invertTree(root.left);
        invertTree(root.right);
        return root;
    }
}
```
* 中序遍历
```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null)
            return null;
        invertTree(root.left);
        TreeNode left = root.left;
        root.left = root.right;
        root.right = left;
        invertTree(root.left);
        return root;
    }
}
```
* 后序遍历
```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null)
            return null;
        invertTree(root.left);
        invertTree(root.right);
        TreeNode left = root.left;
        root.left = root.right;
        root.right = left;
        return root;
    }
}
```
2. 法二（迭代）：BFS，在这里也是层次遍历
```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        Queue<TreeNode> q = new LinkedList<>();
        if(root != null)
            q.add(root);
        while(!q.isEmpty()){
            TreeNode cur = q.poll();
            if(cur != null){
                TreeNode left = cur.left;
                TreeNode right = cur.right;
                cur.left = right;
                cur.right = left;
                q.add(left);
                q.add(right);
            }
        }
        return root;
    }
}
```
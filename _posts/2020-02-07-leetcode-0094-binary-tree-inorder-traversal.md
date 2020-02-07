---
layout: post
title: LeetCode 0094 题解
description: "二叉树的中序遍历"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

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
2. 法二（用栈实现DFS）
```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if(root == null)
            return res;
        TreeNode node = root;
        Stack<TreeNode> s = new Stack<>();
        while(node != null || !s.isEmpty()){
            while(node != null){
                s.push(node);
                node = node.left;
            }
            node = s.pop();
            res.add(node.val);
            node = node.right;
        }
        return res;
    }
}
```
3. 法三（Morris遍历）
```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if(root == null)
            return res;
        TreeNode node = root;
        while(node != null){
            if(node.left == null){
                res.add(node.val);
                node = node.right;            
            }
            else{
                TreeNode pre = node.left;
                while(pre.right != null && pre.right != node)
                    pre = pre.right;
                if(pre.right == null){
                    pre.right = node;
                    node = node.left;
                }
                else{
                    res.add(node.val);
                    pre.right = null;
                    node = node.right;
                }
            }
        }
        return res;
    }
}
```
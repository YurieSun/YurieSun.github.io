---
layout: post
title: LeetCode 0230 题解
description: "二叉搜索树中第K小的元素"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[二叉搜索树中第K小的元素](https://leetcode-cn.com/problems/trim-a-binary-search-tree/comments/)

### 题解
1. 法一（利用中序遍历）：对BST进行中序遍历是有序的，因此，可实现中序遍历，并返回集合中第`k-1`个数。在此基础上，可进行简化：不将已经遍历过的数都存储起来，且不需要遍历所有节点；则将遍历节点进行计数，到达第`k`个数返回即可。
* 递归的中序遍历
```java
class Solution {
    private int cnt, res;
    public int kthSmallest(TreeNode root, int k) {
        if(root ==null)
            return 0;
        cnt = k;
        dfs(root);
        return res;
    }
    private void dfs(TreeNode root){
        if(root != null){
            dfs(root.left);
            if(--cnt == 0)
                res = root.val;
            dfs(root.right);
        }
    }
}
```
* 迭代的中序遍历
```java
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        if(root ==null)
            return 0;
        Stack<TreeNode> s = new Stack<>();
        int cnt = 0, res = 0;
        TreeNode node = root;
        while(true){
            while(node != null){
                s.push(node);
                node = node.left;
            }
            node = s.pop();
            if(--k == 0)
                return node.val;
            node = node.right;    
        }
    }
}
```
2. 法二（递归）
```java
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        int leftCnt = count(root.left);
        if(leftCnt == k - 1)
            return root.val;
        if(leftCnt > k - 1)
            return kthSmallest(root.left, k);
        return kthSmallest(root.right, k - leftCnt - 1);
    }
    private int count(TreeNode root){
        if(root == null)
            return 0;
        return 1 + count(root.left) + count(root.right);
    }
}
```
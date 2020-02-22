---
layout: post
title: 剑指Offer 07 题解
description: "重建二叉树"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[重建二叉树](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)

### 题解
1. 法一（递归）：先通过前序遍历得到根节点，在中序遍历中找到根节点所在位置，从而获得左子树节点个数，这样就知道在前序遍历中哪部分是左子树，哪部分是右子树，并通过递归分别建立左、右子树。
```java
class Solution {
    private Map<Integer, Integer> map;
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if(preorder == null || inorder == null || preorder.length != inorder.length)
            return null;
        map = new HashMap<>();
        for(int i = 0; i < inorder.length; i++)
            map.put(inorder[i], i);
        return reconstructTree(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1);
    }
    private TreeNode reconstructTree(int[] pre, int preL, int preR, int[] in, int inL, int inR){
        if(preL > preR || inL > inR)
            return null;        
        TreeNode root = new TreeNode(pre[preL]);
        int mid = map.get(pre[preL]);
        int leftNum = mid - inL;
        root.left = reconstructTree(pre, preL + 1, preL + leftNum, in, inL, mid - 1);
        root.right = reconstructTree(pre, preL + leftNum + 1, preR, in, mid + 1, inR);
        return root;
    }
}
```
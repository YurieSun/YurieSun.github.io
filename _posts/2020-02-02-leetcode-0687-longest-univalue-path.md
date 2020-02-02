---
layout: post
title: LeetCode 0687 题解
description: "最长同值路径"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[最长同值路径](https://leetcode-cn.com/problems/longest-univalue-path/)

### 题解
1. 法一（递归）
```java
class Solution {
    private int max = 0;
    public int longestUnivaluePath(TreeNode root) {
        depth(root);
        return max;
    }
    private int depth(TreeNode root){
        if(root == null)
            return 0;
        int left = depth(root.left);//指左子树中包含该子树根节点的单侧最长同值路径
        int right = depth(root.right);//指右子树中包含该子树根节点的单侧最长同值路径
        int arrowLeft = 0, arrowRight = 0;
        //若根节点与左节点的值相同，则可将左子树中计算的left加到包含当前根节点的单侧最长同值路径arrowLeft；不相等则将左子树长度视为0。
        //arrowRight同理。
        if(root.left != null && root.val == root.left.val)
            arrowLeft = left + 1;
        if(root.right != null && root.val == root.right.val)
            arrowRight = right + 1;
        //对于每个根节点而言，其真正的最长同值路径为左、右单侧路径相加，在计算过程中保存最大值。
        max = Math.max(max, arrowLeft + arrowRight);
        return Math.max(arrowLeft, arrowRight);
    }
}
```
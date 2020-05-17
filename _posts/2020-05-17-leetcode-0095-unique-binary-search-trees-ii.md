---
layout: post
title: LeetCode 0095 题解
description: "不同的二叉搜索树 II"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[不同的二叉搜索树 II](https://leetcode-cn.com/problems/unique-binary-search-trees-ii/)

### 题解
1. 法一（分治）：对于一棵二叉搜索树，若以`i`作为根节点，则从`1`到`i-1`是它的左子树，从`i+1`到`n`是它的右子树。
```java
class Solution {
    public List<TreeNode> generateTrees(int n) {
        List<TreeNode> res = new ArrayList<>();
        if (n <= 0)
            return res;
        return generate(1, n);

    }
    private List<TreeNode> generate(int start, int end) {
        List<TreeNode> res = new ArrayList<>();
        if (start > end) {
            res.add(null);
            return res;
        }
        for (int i = start; i <= end; i++) {
            List<TreeNode> left = generate(start, i - 1);
            List<TreeNode> right = generate(i + 1, end);
            for (TreeNode l : left) {
                for (TreeNode r : right) {
                    TreeNode root = new TreeNode(i);
                    root.left = l;
                    root.right = r;
                    res.add(root);
                }
            }
        }
        return res;
    }
}
```
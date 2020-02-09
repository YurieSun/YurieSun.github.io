---
layout: post
title: LeetCode 0236 题解
description: "二叉树的最近公共祖先"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

### 题解
1. 法一（递归）：当遇到`p`或`q`或`null`时将会返回，则`left`和`right`的返回值将有上述三种情况。若它们均不为空，说明`p`和`q`分别出现在`root`的左、右子树中，则返回`root`；若`left`为空，说明`p`和`q`均在`root`的右子树上，则返回`right`；`right`同理。
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null || root == p || root == q)
            return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right= lowestCommonAncestor(root.right, p, q);
        if(left != null && right != null)
            return root;
        return left != null ? left : right; 
    }
}
```
* 可简化成以下四行代码：
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null || root == p || root == q) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        return left == null ? right : right == null ? left : root;
    }
}
```
2. 法二（迭代）：对树进行遍历，同时将访问过的节点及其父节点存入哈希表中，建立父亲字典。接下来根据父亲字典从`p`向上查找，并将其沿途经过的节点存入`set`中，这些节点将有可能成为$LCA$。随后根据父亲字典从`q`向上查找，若沿途经过的节点在`set`中存在，说明该节点就是$LCA$。
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        TreeNode node = root;
        while(node != null){
            if(node.val > p.val && node.val > q.val)
                node = node.left;
            else if(node.val < p.val && node.val < q.val)
                node = node.right;
            else
                return node;
        }
        return null;
    }
}
```
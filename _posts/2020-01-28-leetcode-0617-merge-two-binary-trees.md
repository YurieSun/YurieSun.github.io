---
layout: post
title: LeetCode 0617 题解
description: "合并二叉树"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)

### 题解
1. 法一（递归）
```java
class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if(t1 == null)
            return t2;
        if(t2 == null)
            return t1;
        TreeNode root = new TreeNode(t1.val + t2.val);
        root.left = mergeTrees(t1.left, t2.left);
        root.right = mergeTrees(t1.right, t2.right);
        return root;
    }
}
```
2. 法二（迭代）：入栈与出栈的是由两个树相同位置节点所组成的数组。
```java
class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if(t1 == null)
            return t2;
        Stack<TreeNode[]> s = new Stack<>();
        s.push(new TreeNode[]{t1, t2});
        while(!s.isEmpty()){      
            TreeNode[] t = s.pop();
            if(t[0] != null && t[1] != null){
                t[0].val += t[1].val;
                if(t[0].left == null)
                    t[0].left = t[1].left;
                else
                    s.push(new TreeNode[]{t[0].left, t[1].left});
                if(t[0].right == null)
                    t[0].right = t[1].right;
                else
                    s.push(new TreeNode[]{t[0].right, t[1].right});
            }
        }
        return t1;

    }
}
```
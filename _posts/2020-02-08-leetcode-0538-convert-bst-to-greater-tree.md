---
layout: post
title: LeetCode 0538 题解
description: "把二叉搜索树转换为累加树"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/trim-a-binary-search-tree/comments/)

### 题解
1. 法一（递归）：根据right-root-left的顺序遍历BST的节点，同时计算该节点的值。（相当于中序遍历）
* 递归的中序遍历
```java
class Solution {
    private int sum = 0;
    public TreeNode convertBST(TreeNode root) {
        if(root == null)
            return null;
        convertBST(root.right);
        sum += root.val;
        root.val = sum;
        convertBST(root.left);
        return root;
    }
}
```
2. 法二（迭代）：同样是与中序遍历相似的做法。
```java
class Solution {
    public TreeNode convertBST(TreeNode root) {
        if(root == null)
            return null;
        Stack<TreeNode> s = new Stack<>();
        int sum = 0;
        TreeNode node = root;
        while(node != null || !s.isEmpty()){
            while(node != null){
                s.push(node);
                node = node.right;
            }
            node = s.pop();
            sum += node.val;
            node.val = sum;
            node = node.left;
        }
        return root;
    }
}
```
3. 法三（Morris遍历）：使用Morris遍历实现中序遍历时，节点访问顺序为left-root-right，而这里的访问顺序为right-root-left，因此只需将中序遍历中的所有left变成right，所有right变成left，且在原先输出节点的地方改变节点数值即可。
```java
class Solution {
    public TreeNode convertBST(TreeNode root) {
        if(root == null)
            return null;
        TreeNode node = root;
        int sum = 0;
        while(node != null){
            if(node.right == null){
                sum += node.val;
                node.val = sum;
                node = node.left;
            }
            else{
                TreeNode pre = node.right;
                while(pre.left != null && pre.left != node)
                    pre = pre.left;
                if(pre.left == null){
                    pre.left = node;
                    node = node.right;
                }
                else{
                    sum += node.val;
                    node.val = sum;
                    pre.left = null;
                    node = node.left;
                }
            }
        }
        return root;
    }
}
```
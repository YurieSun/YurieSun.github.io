---
layout: post
title: LeetCode 0144 题解
description: "二叉树的前序遍历"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

### 题解
1. 法一（递归）
```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        dfs(root, res);
        return res;
    }
    private void dfs(TreeNode root, List<Integer> list){
        if(root != null){
            list.add(root.val);
            dfs(root.left, list);
            dfs(root.right, list);
        }
    }
}
```
2. 法二（用栈实现DFS）
```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if(root == null)
            return res;
        Stack<TreeNode> s = new Stack<>();
        s.push(root);
        while(!s.isEmpty()){
            TreeNode node = s.pop();
            res.add(node.val);
            //先右后左，保证先访问左节点
            if(node.right != null)
                s.push(node.right);
            if(node.left != null)
                s.push(node.left);
        }
        return res;
    }
}
```
3. 法三（Morris遍历）
```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if(root == null)
            return res;
        TreeNode node = root;
        while(node != null){
            //左顾：若左节点为空，则打印当前节点，并处理右节点。
            if(node.left == null){
                res.add(node.val);
                node = node.right;
            }
            //右盼：处理右节点
            else{
                //这里的循环用来找根节点的前序节点（先走到左节点，再往右边一直走到底）
                //前序节点的右指针一定为空。
                TreeNode pre = root.left;
                while(pre.right != null && pre.right != node)
                    pre = pre.right;
                //若前序节点的右指针为空，则打印当前节点，并将前序节点的右指针指向当前节点，再进入当前节点的左节点。
                if(pre.right == null){
                    res.add(node.val);
                    pre.right = node;
                    node = node.left;
                }
                //若前序节点的右指针指向当前节点，则将当前节点的右指针设为空，再进入当前节点的右节点。
                else{
                    pre.right = null;
                    node = node.right;
                }
            }
        }
        return res;
    }
}
```
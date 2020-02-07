---
layout: post
title: LeetCode 0145 题解
description: "二叉树的后序遍历"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)

### 题解
1. 法一（递归）
```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        dfs(root, res);
        return res;
    }
    private void dfs(TreeNode root, List<Integer> list){
        if(root != null){
            dfs(root.left, list);
            dfs(root.right, list);
            list.add(root.val);
        }
    }
}
```
2. 法二（用栈实现DFS）：后序遍历是left-right-root，则可先通过前序遍历实现root-right-left，再将结果链表逆序即可。
```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if(root == null)
            return res;
        Stack<TreeNode> s = new Stack<>();
        s.push(root);
        while(!s.isEmpty()){
            TreeNode node = s.pop();
            res.add(node.val);
            if(node.left != null)
                s.push(node.left);
            if(node.right != null)
                s.push(node.right);
        }
        Collections.reverse(res);//逆序输出
        return res;
    }
}
```
* 用LinkedList：通过前序遍历实现root-right-left，如果用链表存储，并且通过`addFirst`或`add(0, val)`将节点加到链表头，就可以不使用反转了。
```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        LinkedList<Integer> res = new LinkedList<>();
        if(root == null)
            return res;
        Stack<TreeNode> s = new Stack<>();
        s.push(root);
        while(!s.isEmpty()){
            TreeNode node = s.pop();
            //或者res.add(0, root.val);
            res.addFirst(node.val);
            if(node.left != null)
                s.push(node.left);
            if(node.right != null)
                s.push(node.right);
        }
        return res;
    }
}
```
3. 法三（栈）：该方法与中序遍历类似。用栈存储未访问的右节点，并设置`last`存储上次访问过的节点，若当前节点的右节点已被访问过，则可输出当前节点。
```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if(root == null)
            return res;
        Stack<TreeNode> s = new Stack<>();
        TreeNode node = root;
        TreeNode last = null;
        while(node != null || !s.isEmpty()){
            while(node != null){
                s.push(node);
                node = node.left;
            }
            node = s.peek();
            //当前节点无右节点或者右节点已访问过，则可输出当前节点
            if(node.right == null || node.right == last){
                res.add(node.val);
                s.pop();
                last = node;
                node = null;     
            }     
            else
                node = node.right;
        }
        return res;
    }
}
```
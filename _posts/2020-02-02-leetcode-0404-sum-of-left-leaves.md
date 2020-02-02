---
layout: post
title: LeetCode 0404 题解
description: "左叶子之和"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[左叶子之和](https://leetcode-cn.com/problems/sum-of-left-leaves/)

### 题解
1. 法一（递归）
```java
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        if(root == null)
            return 0;
        int sum = 0;
        if(isLeaf(root.left))
            return root.left.val + sumOfLeftLeaves(root.right);
        return sumOfLeftLeaves(root.left) + sumOfLeftLeaves(root.right);
    }
    private boolean isLeaf(TreeNode node){
        if(node == null)
            return false;
        return node.left == null && node.right == null;
    }
}
```
2. 法二（迭代）
* 栈实现DFS
```java
import javafx.util.Pair;
//通过增加boolean来判断节点是否为左节点。
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        if(root == null)
            return 0;
        int sum = 0;
        Stack<Pair<TreeNode, Boolean>> s = new Stack<>();
        s.push(new Pair(root, false));
        while(!s.isEmpty()){
            Pair<TreeNode, Boolean> cur = s.pop();
            root = cur.getKey();
            boolean flag = cur.getValue();
            if(flag && root.left == null && root.right == null)
                sum += root.val;
            if(root.left != null)
                s.push(new Pair(root.left, true));
            if(root.right != null)
                s.push(new Pair(root.right, false));
        }
        return sum;
    }
}
```
* 同理可用队列实现BFS
---
layout: post
title: LeetCode 0111 题解
description: "二叉树的最小深度"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)

### 题解
1. 法一（递归）
```java
class Solution {
    public int minDepth(TreeNode root) {
        if(root == null)
            return 0;
        // 分成三种情况：
        //(1)若root左、右节点均为空，说明是叶子结点，返回1；
        //(2)若root左、右节点有一个为空，则不是叶子结点，返回非空节点的最小深度；
        //(3)若root左、右节点均不为空，返回左、右节点中最小深度的较小值。
        int left = minDepth(root.left);
        int right = minDepth(root.right);
        //情况(1)和(2)
        if(left == 0 || right == 0)
            return left + right + 1;
        //情况(3)
        return Math.min(left, right) + 1;
    }
}
```
2. 法二（迭代）
* 栈实现DFS
```java
import javafx.util.Pair;
class Solution {
    public int minDepth(TreeNode root) {
        if(root == null)
            return 0;
        int count = Integer.MAX_VALUE;
        Stack<Pair<TreeNode, Integer>> s = new Stack<>();
        s.push(new Pair(root ,1));
        while(!s.isEmpty()){
            Pair<TreeNode, Integer> cur = s.pop();
            root = cur.getKey();
            int cnt = cur.getValue();
            if(root.left == null && root.right == null)
                count = Math.min(cnt, count);
            if(root.left != null)
                s.push(new Pair(root.left, cnt + 1));
            if(root.right != null)
                s.push(new Pair(root.right, cnt + 1));
        }
        return count;
    }
}
```
* 队列实现BFS
```java
import javafx.util.Pair;
class Solution {
    public int minDepth(TreeNode root) {
        if(root == null)
            return 0;
        int count = 0;
        LinkedList<Pair<TreeNode, Integer>> q = new LinkedList<>();
        q.add(new Pair(root, 1));
        while(!q.isEmpty()){
            Pair<TreeNode, Integer> cur = q.poll();
            root = cur.getKey();
            count = cur.getValue();
            if(root.left == null && root.right == null)
                //由于是BFS，那么一旦遇到叶子结点，就是离根节点最近的，可直接返回深度；也不需要与当前深度进行比较。
                break;
            if(root.left != null)
                q.add(new Pair(root.left, count + 1));
            if(root.right != null)
                q.add(new Pair(root.right, count + 1));  
        }
        return count;
    }
}
```
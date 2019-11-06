---
layout: post
title: LeetCode 0101 题解
description: "二叉树的最大深度"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

### 思路
递归：根节点的深度就等于左/右子树深度的较大值再加1，当左、右节点均为空时，返回0。

### 题解
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null) return 0;
        return 1+Math.max(maxDepth(root.left),maxDepth(root.right));
    }
}
```
* 法二：用队列实现BFS
```java
import javafx.util.Pair; //要将Pair类导入，或自己定义。
class Solution {
    public boolean isSymmetric(TreeNode root) {
		Queue<Pair<TreeNode, Integer>> q = new LinkedList<>();
		if(root != null)
            q.add(new Pair(root, 1));
        int depth = 0;
        while(!q.isEmpty()){
            Pair<TreeNode, Integer> cur = q.poll();
            root = cur.getKey();
            int d = cur.getValue();
            if(root != null){
                depth = Math.max(depth, d);
                q.add(new Pair(root.left, d + 1));
                q.add(new Pair(root.right, d + 1));
            }
        }
        return depth;
    }
}
```
* 法三：用栈实现DFS
```java
import javafx.util.Pair;
class Solution {
    public boolean isSymmetric(TreeNode root) {
		Stack<Pair<TreeNode, Integer>> s = new Stack<>();
        if(root != null)
            s.add(new Pair(root, 1));
        int depth = 0;
        while(!s.isEmpty()){
            Pair<TreeNode, Integer> cur = s.pop(); 
            root = cur.getKey();
            int d = cur.getValue();
            if(root != null){
                depth = Math.max(depth, d);
                s.push(new Pair(root.left, d + 1));
                s.push(new Pair(root.right, d + 1));
            }
        }
        return depth;
    }
}
```
### 思考
* 递归  
时间复杂度：$O(N)$ 
空间复杂度：最好情况是完全二叉树，此时为$O(\log N)$；最坏情况是完全不平衡的树，相当于线性表结构，此时为$O(N)$。
* BFS和DFS  
时间复杂度：$O(N)$ 
空间复杂度：$O(N)$
---
layout: post
title: LeetCode 0101 题解
description: "对称二叉树"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)

### 思路
递归：先查看根节点的值是否相等，随后分别查看$A$的左子树和$B$的右子树、$A$的右子树和$B$的左子树是否对称，直到遇到空节点；在查看过程中一旦遇到不对称的情况即返回。

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
    public boolean isSymmetric(TreeNode root) {
        return isSymmetric(root, root);
    }
	public boolean isSymmetric(TreeNode p, TreeNode q) {
        if(p == null && q == null) return true;
        if(p == null || q == null) return false;
        return p.val == q.val && isSymmetric(p.left, q.right) && isSymmetric(p.right, q.left);
    }
}
```
* 也可写成：
```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if(root == null)
            return true;
        return isSymmetric(root.left, root.right);
    }
    private boolean isSymmetric(TreeNode s, TreeNode t){
        if(s == null && t == null)
            return true;
        if(s == null || t == null)
            return false;
        if(s.val != t.val)
            return false;
        return isSymmetric(s.left, t.right) && isSymmetric(s.right, t.left);
    }
}
```
* 法二：迭代（将节点依次放入队列中，每次将两个节点出队，判断是否相等，再把$A$的左节点和$B$右节点入队，直到遇到空节点；在查看过程中一旦遇到不相等的元素即返回。
```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
		Queue<TreeNode> q = new LinkedList<TreeNode>();
		q.add(root);
		q.add(root);
		while(!q.isEmpty()) {
			TreeNode t1 = q.poll();
			TreeNode t2 = q.poll();
			if(t1 == null && t2 == null) continue;
			if(t1 == null || t2 == null) return false;
			if(t1.val != t2.val) return false;
			q.add(t1.left);
			q.add(t2.right);
			q.add(t1.right);
			q.add(t2.left);
		}
        return true;
    }
}
```
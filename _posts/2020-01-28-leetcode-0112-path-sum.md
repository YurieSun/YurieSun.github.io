---
layout: post
title: LeetCode 0112 题解
description: "路径总和"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[路径总和](https://leetcode-cn.com/problems/path-sum/)

### 题解
1. 法一（递归）
```java
class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        if(root == null)
            return false;
        if(root.left == null && root.right == null && root.val == sum)
            return true;
        return hasPathSum(root.left, sum - root.val) || hasPathSum(root.right, sum - root.val);
    }
}
```
2. 法二（迭代）：将每个节点和其应该得到的值组成`Pair`来进行入栈和出栈的操作。若到达叶子结点且其值为0，则说明找到了符合题意的路径，返回`true`；若遍历完仍未找到则返回`false`；
```java
import javafx.util.Pair;
class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        if(root == null)
            return false;
        Stack<Pair<TreeNode, Integer>> s = new Stack<>();
        s.push(new Pair(root, sum - root.val));
        while(!s.isEmpty()){
            Pair<TreeNode, Integer> cur = s.pop();
            TreeNode t = cur.getKey();
            int num = cur.getValue();
            if(t.left == null && t.right == null && num == 0)
                return true;
            if(t.left != null)
                s.push(new Pair(t.left, num - t.left.val));
            if(t.right != null)
                s.push(new Pair(t.right, num - t.right.val));
        }
        return false;
    }
}
```
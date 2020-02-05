---
layout: post
title: LeetCode 0337 题解
description: "打家劫舍 III"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[打家劫舍 III](https://leetcode-cn.com/problems/house-robber-iii/)

### 题解
1. 法一（递归）
```java
class Solution {
    public int rob(TreeNode root) {
        if(root == null)
            return 0;
        //sum1为根节点与其孙子节点之和，sum2为根节点的子节点之和。
        //并不是间隔层求和。
        int sum1 = root.val, sum2 = 0;
        if(root.left != null)
            sum1 += rob(root.left.left) + rob(root.left.right);
        if(root.right != null)
            sum1 += rob(root.right.left) + rob(root.right.right);
        sum2 += rob(root.left) + rob(root.right);
        return Math.max(sum1, sum2);
    }
}
```
* 改进一：上述递归过程中有许多重复计算，则采用记忆化将计算过的值存储下来。树不好用数组进行存放，则采用哈希表。
```java
class Solution {
    public int rob(TreeNode root) {
        HashMap<TreeNode, Integer> memo = new HashMap<>();
        return myRob(root, memo);
    }
    private int myRob(TreeNode root, HashMap<TreeNode, Integer> memo){
        if(root == null)
            return 0;
        if(memo.containsKey(root))
            return memo.get(root);
        int sum1 = root.val, sum2 = 0;
        if(root.left != null)
            sum1 += myRob(root.left.left, memo) + myRob(root.left.right, memo);
        if(root.right != null)
            sum1 += myRob(root.right.left, memo) + myRob(root.right.right, memo);
        sum2 += myRob(root.left, memo) + myRob(root.right, memo);
        int result = Math.max(sum1, sum2);
        memo.put(root, result);
        return result;
    }
}
```
* 改进二：通过动态规划的思路描述状态转移，节约了上述解法中哈希表所使用的的空间。
```java
class Solution {
    public int rob(TreeNode root) {
        int[] result = myRob(root);
        return Math.max(result[0], result[1]);
    }
    private int[] myRob(TreeNode root){
        if(root == null)
            return new int[2];
        int[] result = new int[2];
        int[] left = myRob(root.left);
        int[] right = myRob(root.right);
        //0表示不抢，1表示抢。
        result[0] = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
        result[1] = left[0] + right[0] + root.val;
        return result;
    }
}
```
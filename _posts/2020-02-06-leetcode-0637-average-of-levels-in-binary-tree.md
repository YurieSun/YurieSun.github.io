---
layout: post
title: LeetCode 0637 题解
description: "二叉树的层平均值"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[二叉树的层平均值](https://leetcode-cn.com/problems/average-of-levels-in-binary-tree/)

### 题解
1. 法一（层次遍历）：通过队列实现BFS（层次遍历）。当开始访问某一层的节点时，队列中所剩下的节点数即为当前层的节点个数，由此可以控制每一层的节点访问。
```java
class Solution {
    public List<Double> averageOfLevels(TreeNode root) {
        List<Double> result = new ArrayList<>();
        if(root == null)
            return result;
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        while(!q.isEmpty()){
            int count = q.size();
            double sum = 0;
            for(int i = 0; i < count; i++){
                TreeNode cur = q.poll();
                sum += cur.val;
                if(cur.left != null)
                    q.add(cur.left);
                if(cur.right != null)
                    q.add(cur.right);
            }
            result.add(sum/count);
        }
        return result;
    }
}
```
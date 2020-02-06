---
layout: post
title: LeetCode 0513 题解
description: "找树左下角的值"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[找树左下角的值](https://leetcode-cn.com/problems/find-bottom-left-tree-value/)

### 题解
1. 法一（层次遍历）：通常层次遍历时先访问左节点，再访问右节点；在这里如果先访问右节点，再访问左节点，所访问的最后一个节点就是左下角的节点。
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
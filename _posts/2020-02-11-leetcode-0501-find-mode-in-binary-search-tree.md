---
layout: post
title: LeetCode 0501 题解
description: "二叉搜索树中的众数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[二叉搜索树中的众数](https://leetcode-cn.com/problems/find-mode-in-binary-search-tree/)

### 题解
1. 法一（中序遍历）：对BST进行中序遍历得到的是有序序列，因此在遍历过程中只有相邻节点才有可能相等，则将问题转变成求有序数组中的众数。因此在中序遍历的过程中记录该值的出现次数`curTime`，若比`maxTime`大，则清空结果数组，并将当前值放入空数组中，更新`maxTime`；若`curTime`等于`maxTime`，则将当前值放入结果数组中即可。
```java
class Solution {
    int maxTime = 1;
    int curTime = 1;
    TreeNode pre = null;
    public int[] findMode(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        inOrder(root, list);
        int[] res = new int[list.size()];
        for(int i = 0; i < list.size(); i++)
            res[i] = list.get(i);
        return res;
    }
    private void inOrder(TreeNode root, List<Integer> list){
        if(root == null)
            return;
        inOrder(root.left, list);
        if(pre != null){
            if(pre.val == root.val)
                curTime++;
            else
                curTime = 1;
        }
        if(curTime > maxTime){
            list.clear();
            list.add(root.val);
            maxTime = curTime;
        }
        else if(curTime == maxTime)
            list.add(root.val);
        pre = root;
        inOrder(root.right, list);
    }
}
```
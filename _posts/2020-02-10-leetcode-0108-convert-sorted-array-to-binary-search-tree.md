---
layout: post
title: LeetCode 0108 题解
description: "将有序数组转换为二叉搜索树"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)

### 题解
1. 法一（递归）
```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return toBST(nums, 0, nums.length - 1);
    }
    private TreeNode toBST(int[] nums, int start, int end){
        if(start > end)
            return null;
        int mid = start + (end - start) / 2;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = toBST(nums, start, mid - 1);
        root.right = toBST(nums, mid + 1, end);
        return root;
    }
}
```
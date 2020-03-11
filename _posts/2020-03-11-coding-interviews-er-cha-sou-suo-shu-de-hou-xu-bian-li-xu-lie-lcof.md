---
layout: post
title: 剑指Offer 33 题解
description: "二叉搜索树的后序遍历序列"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[二叉搜索树的后序遍历序列](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)

### 题解
1. 法一（递归）：根据后序遍历的顺序，数组最后一个一定是根节点，则通过与根节点的比较可以划分出左右子树。若在右子树对应的位置出现了比根节点小的数，则返回`false`，并递归检验左、右子树。
```java
class Solution {
    public boolean verifyPostorder(int[] postorder) {
        if (postorder == null || postorder.length == 0)
            return true;
        return verify(postorder, 0, postorder.length - 1);
    }

    private boolean verify(int[] nums, int left, int right) {
        if (right - left <= 1)
            return true;
        // int root = right;
        int cut = left;
        while (cut < right && nums[cut] <= nums[right])
            cut++;
        for (int i = cut; i < right; i++) {
            if (nums[i] < nums[right])
                return false;
        }
        return verify(nums, left, cut - 1) && verify(nums, cut, right - 1);
    }
}
```
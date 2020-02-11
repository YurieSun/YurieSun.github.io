---
layout: post
title: LeetCode 0653 题解
description: "两数之和 IV - 输入 BST"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[两数之和 IV - 输入 BST](https://leetcode-cn.com/problems/two-sum-iv-input-is-a-bst/)

### 题解
1. 法一（中序遍历 + 双指针）：其中中序遍历使用递归或迭代均可。
```java
class Solution {
    public boolean findTarget(TreeNode root, int k) {
        List<TreeNode> nums = new ArrayList<>();
        Stack<TreeNode> s = new Stack<>();
        while(root != null || !s.isEmpty()){
            while(root != null){
                s.push(root);
                root = root.left;
            }
            root = s.pop();
            nums.add(root.val);
            root = root.right;
        }
        int i = 0; j = nums.size();
        while(i < j){
            if(nums[i] + nums[j] == k)
                return true;
            else if(nums[i] + nums[j] > k)
                j--;
            else
                i++;
        }
        return false;
    }
}
```
2. 法二：遍历BST的过程中利用哈希表存储节点值。这里的遍历方法可以采用递归或迭代。
```java
class Solution {
    public boolean findTarget(TreeNode root, int k) {
        Set<Integer> set = new HashSet<>();
        return find(root, k, set);
    }
    private boolean find(TreeNode root, int k, Set<Integer> set){
        if(root == null)
            return false;
        if(set.contains(k - root.val))
            return true;
        set.add(root.val);
        return find(root.left, k, set) || find(root.right, k, set);
    }
}
```
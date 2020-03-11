---
layout: post
title: 剑指Offer 32-III 题解
description: "从上到下打印二叉树 III"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[从上到下打印二叉树 III](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)

### 题解
1. 法一：和[从上到下打印二叉树 II](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)基本相同，只不过在偶数层输出后进行反转。
```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null)
            return res;
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        while (!q.isEmpty()) {
            int cnt = q.size();
            List<Integer> list = new ArrayList<>();
            while (cnt-- > 0) {
                TreeNode cur = q.poll();
                list.add(cur.val);
                if (cur.left != null)
                    q.add(cur.left);
                if (cur.right != null)
                    q.add(cur.right);
            }
            // 若res大小为奇数，说明现在处理的是偶数层，需要反转
            if ((res.size() & 1) != 0)
                Collections.reverse(list);
            res.add(list);
        }
        return res;
    }
}
```
2. 法二：不使用`reserve()`函数，通过双向队列分奇偶层进行操作。
```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null)
            return res;
        LinkedList<TreeNode> q = new LinkedList<>();
        q.add(root);
        int height = 0;
        while (!q.isEmpty()) {
            int cnt = q.size();
            height++;
            List<Integer> list = new ArrayList<>();
            Stack<TreeNode> s = new Stack<>();
            while (cnt-- > 0) {
                TreeNode cur;
                // 奇数层：正常头部出队，尾部入队
                if ((height & 1) != 0) {
                    cur = q.poll();
                    if (cur.left != null)
                        q.add(cur.left);
                    if (cur.right != null)
                        q.add(cur.right);
                }
                // 偶数层：尾部出队，头部逆序入队
                else {
                    cur = q.pollLast();
                    if (cur.right != null)
                        q.addFirst(cur.right);
                    if (cur.left != null)
                        q.addFirst(cur.left);
                }
                list.add(cur.val);
            }
            res.add(list);
        }
        return res;
    }
}
```
---
layout: post
title: LeetCode 0297 题解
description: "二叉树的序列化与反序列化"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[二叉树的序列化与反序列化](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)

### 题解
1. 法一（层次遍历）
```java
public class Codec {
    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if (root == null)
            return "";
        StringBuilder res = new StringBuilder();
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        while (!q.isEmpty()) {
            TreeNode cur = q.poll();
            if (cur != null) {
                res.append(cur.val).append(",");
                q.add(cur.left);
                q.add(cur.right);
            } else
                res.append("#,");
        }
        return res.toString();
    }
    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if (data == null || data.length() == 0)
            return null;
        Queue<TreeNode> list = new LinkedList<>();
        String[] values = data.split(",");
        TreeNode root = createNode(values[0]);
        list.add(root);
        int rootIndex = 0;
        int valueIndex = 1;
        while (rootIndex < list.size()) {
            TreeNode node = list.poll();
            if (valueIndex < values.length) {
                node.left = createNode(values[valueIndex++]);
                node.right = createNode(values[valueIndex++]);
            }
            if (node.left != null)
                list.add(node.left);
            if (node.right != null)
                list.add(node.right);
        }
        return root;
    }
    private TreeNode createNode(String s) {
        if (s == null || s.equals("#"))
            return null;
        return new TreeNode(Integer.parseInt(s));

    }
}
// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```
2. 法二（DFS）
```java
public class Codec {
    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if (root == null)
            return "";
        StringBuilder res = preOrderSerialize(root, new StringBuilder());
        return res.toString();
    }
    private StringBuilder preOrderSerialize(TreeNode root, StringBuilder s) {
        if (root != null) {
            s.append(root.val).append(",");
            preOrderSerialize(root.left, s);
            preOrderSerialize(root.right, s);
        } else
            s.append("#,");
        return s;
    }
    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if (data == null || data.length() == 0)
            return null;
        String[] values = data.split(",");
        List<String> list = new LinkedList<>(Arrays.asList(values));
        return preOrderDeserialize(list);
    }

    private TreeNode preOrderDeserialize(List<String> list) {
        if (list.get(0).equals("#")) {
            list.remove(0);
            return null;
        }
        TreeNode root = new TreeNode(Integer.valueOf(list.get(0)));
        list.remove(0);
        root.left = preOrderDeserialize(list);
        root.right = preOrderDeserialize(list);
        return root;
    }
}
```
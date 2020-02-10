---
layout: post
title: LeetCode 0109 题解
description: "将有序链表转换为二叉搜索树"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[将有序链表转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/)

### 题解
1. 法一（递归）：遍历链表并将节点按顺序存储在数组中，再利用第 [108题](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/) 的解法将有序数组转换成BST。
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
* 递归的另一种做法：先通过快慢指针找到链表中间的节点`mid`，作为树的根节点；并将链表从`mid`左侧分成两部分，并进行递归分别作为根节点的左、右子树。（空间复杂度降低，但时间复杂度提高）
```java
class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        if(head == null)
            return null;
        if(head.next == null)
            return new TreeNode(head.val);
        ListNode pre = preMid(head);
        ListNode mid = pre.next;
        pre.next = null;
        TreeNode root = new TreeNode(mid.val);
        root.left = sortedListToBST(head);
        root.right = sortedListToBST(mid.next);
        return root;
    }
    private ListNode preMid(ListNode head){
        ListNode slow = head, fast = head.next;
        ListNode pre = head;
        while(fast != null && fast.next != null){
            pre = slow;
            slow = slow.next;
            fast = fast.next.next;
        }
        return pre;
    }
}
```
* （递归）通过递归的中序遍历，自下往上建立BST，构建树所遍历的节点顺序与链表的节点顺序一致。
```java
class Solution {
    ListNode node;
    public TreeNode sortedListToBST(ListNode head) {
        if(head == null)
            return null;
        node = head;
        ListNode cur = head;
        int cnt = 0;
        while(cur != null){
            cnt++;
            cur = cur.next;
        }
        return toBST(0, cnt - 1);
    }
    private TreeNode toBST(int start, int end){
        if(start > end)
            return null;
        int mid = start + (end - start) / 2;
        TreeNode left = toBST(start, mid - 1);
        TreeNode root = new TreeNode(node.val);
        root.left = left;
        node = node.next;
        root.right = toBST(mid + 1, end);
        return root;
    }
}
```

---
layout: post
title: LeetCode 0092 题解
description: "反转链表 II"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)

### 题解
1. 法一：遍历链表，找到需要反转的一段链表，像反转整个链表一样进行反转，再将反转后的链表与之前的前一段链表和后一段链表相连。
```java
class Solution {
    public ListNode reverseBetween(ListNode head, int m, int n) {
        if (head == null || head.next == null)
            return head;
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode node = dummy;
        // node是反转起始节点的前一个节点
        for (int i = 1; i < m; i++) {
            node = node.next;
        }
        // cur是反转起始节点
        ListNode cur = node.next;
        ListNode pre = null;
        ListNode next = null;
        // 反转链表
        for (int i = m; i <= n; i++) {
            next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        // 将反转后的链表与前一段和后一段连接起来
        node.next.next = cur;
        node.next = pre;
        return dummy.next;
    }
}
```
2. 法二（头插法）：找到反转起始节点的前一个节点`pre`和起始节点`cur`，通过将`cur.next`不断插入到`pre`的后面去，就可以实现反转。
```java
class Solution {
    public ListNode reverseBetween(ListNode head, int m, int n) {
        if (head == null || head.next == null)
            return head;
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode pre = dummy;
        // pre是反转起始节点的前一个节点
        for (int i = 1; i < m; i++) {
            pre = pre.next;
        }
        ListNode cur = pre.next;
        // 注意这里的终止条件为i < n，因为只将一个节点进行头插，其实已经反转了两个节点。
        for (int i = m; i < n; i++) {
            // 将next插入到pre后面
            ListNode next = cur.next;
            cur.next = next.next;
            next.next = pre.next;
            pre.next = next;
        }
        return dummy.next;
    }
}
```
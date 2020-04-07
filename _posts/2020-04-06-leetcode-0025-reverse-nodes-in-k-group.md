---
layout: post
title: LeetCode 0025 题解
description: "K 个一组翻转链表"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

### 题解
1. 法一：反转每一小段链表，并与前一段和后一段连接起来。
```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        if (head == null || head.next == null)
            return head;
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        // start表示反转起始节点，end表示反转结束节点；
        // pre表示反转起始节点的前一个节点，next表示反转结束节点的后一个节点。
        // 即 pre -> start -> ... -> end -> next
        ListNode pre = dummy;
        ListNode end = pre;
        while (end.next != null) {
            // 找到end节点位置
            for (int i = 0; i < k && end != null; i++)
                end = end.next;
            // 说明这一段链表的长度小于k，不需要反转。
            if (end == null)
                break;
            ListNode start = pre.next;
            ListNode next = end.next;
            // 注意要将当前反转链表和后面断开，否则会将后面的也反转了。
            end.next = null;
            pre.next = reverse(start);
            start.next = next;
            pre = start;
            end = pre;
        }
        return dummy.next;
    }
    // 反转以head为头节点的链表
    private ListNode reverse(ListNode head) {
        if (head == null || head.next == null)
            return head;
        ListNode pre = null;
        ListNode next = null;
        while (head != null) {
            next = head.next;
            head.next = pre;
            pre = head;
            head = next;
        }
        return pre;
    }
}
```
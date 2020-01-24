---
layout: post
title: LeetCode 0024 题解
description: "两两交换链表中的节点"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

### 题解
1. 法一（递归法）
```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if(head == null || head.next == null)
            return head;
        ListNode next = head.next;
        head.next = swapPairs(next.next);
        next.next = head;
        return next;
    }
}
```
2. 法二（迭代法）
```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode cur = dummy;
        while(cur.next != null && cur.next.next != null){
            ListNode start = cur.next, end = cur.next.next;
            start.next = end.next;
            end.next = start;
            cur.next = end;
            cur = start;
        }
        return dummy.next;
    }
}
```
---
layout: post
title: LeetCode 0206 题解
description: "反转链表"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

### 题解
1. 法一（递归法）
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null)
            return head;
        ListNode next = head.next;
        ListNode newHead = reverseList(next);
        next.next = head;
        head.next = null;
        return newHead;
    }
}
```
2. 法二（迭代法）
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        while(cur != null){
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
}
```
3. 法三（头插法）
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode dummy = new ListNode(-1);
        while(head != null){
            ListNode next = head.next;
            head.next = dummy.next;
            dummy.next = head;
            head = next;
        }
        return dummy.next;
    }
}
```
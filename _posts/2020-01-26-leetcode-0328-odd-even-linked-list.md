---
layout: post
title: LeetCode 0328 题解
description: "奇偶链表"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list/)

### 题解
1. 法一：遍历链表，并分别将奇偶节点放到对应的链表中，再将偶链表连接到奇链表后面。（不满足空间复杂度为 $O(1)$ ）
```java
class Solution {
    public ListNode oddEvenList(ListNode head) {
        if(head == null || head.next == null)
            return head;
        ListNode odd = head, even = head.next;
        ListNode oddHead = odd, evenHead = even;
        while(head.next != null && head.next.next != null){
            head = head.next.next;
            odd.next = head;
            even.next = head.next;
            odd = odd.next;
            even = even.next;
        }
        odd.next = evenHead;
        return oddHead;
    }
}
```
* 更简洁的写法
```java
class Solution {
    public ListNode oddEvenList(ListNode head) {
        if(head == null)
            return head;
        ListNode odd = head, even = odd.next;
        ListNode evenHead = even;
        while(even != null && even.next != null){
            odd.next = odd.next.next;
            odd = odd.next;
            even.next = even.next.next;
            even = even.next;
        }
        odd.next = evenHead;
        return head;
    }
}
```
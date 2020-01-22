---
layout: post
title: LeetCode 0019 题解
description: "删除链表的倒数第N个节点"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

### 题解
1. 法一（两次遍历）：第一次遍历统计链表长度，第二次遍历实现删除节点。
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode newHead = new ListNode(-1);
        newHead.next = head;
        ListNode cur = head;
        int count = 0;
        while(cur != null){
            count++;
            cur = cur.next;
        }
        count -= n;
        cur = newHead;
        while(count > 0){
            count--;
            cur = cur.next;
        }
        cur.next = cur.next.next;
        return newHead.next;
    }
}
```
2. 法二（双指针法）：设置两个指针`p`与`q`，当`q=null`且`p`与`q`之间间隔的元素个数为`n`时，`p`所指的下一个节点就是要删除的节点。因此先让指针`q`走`n+1`步，然后让`p`和`q`一起走，使得两指针之间的元素个数保持为`n`，则当`q=null`时，将`p`指向下下个节点便可将其下一个节点删除。
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode newHead = new ListNode(-1);
        newHead.next = head;
        ListNode p = newHead;
        ListNode q = newHead;
        while(n >= 0){
            q = q.next;
            n--;
        }
        while(q != null){
            p = p.next;
            q = q.next;
        }
        p.next = p.next.next;
        return newHead.next;
    }
}
```
* 设置第一个哑节点的目的是为了防止删除头节点或者只有一个节点时，导致`head`节点为空，因此不能返回`head`节点，而是通过设置哑结点直接返回其后一个节点。
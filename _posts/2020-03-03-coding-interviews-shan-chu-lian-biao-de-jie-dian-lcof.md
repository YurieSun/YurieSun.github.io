---
layout: post
title: 剑指Offer 18 题解
description: "删除链表的节点"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[删除链表的节点](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

### 题解
1. 法一：若删除的是头节点，直接返回后一节点，否则进入循环。
```java
class Solution {
    public ListNode deleteNode(ListNode head, int val) {
        if(head.val == val)
            return head.next;
        ListNode pre = head;
        ListNode cur = head.next;
        while(cur != null){
            if(cur.val == val){
                pre.next = cur.next;
                break;
            }
            pre = cur;
            cur = cur.next;
        }
        return head;
    }
}
```
* 通过构造头部假节点，可以不讨论删除头节点这种特殊情况。
```java
class Solution {
    public ListNode deleteNode(ListNode head, int val) {
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode pre = dummy;
        ListNode cur = head;
        while(cur != null){
            if(cur.val == val){
                pre.next = cur.next;
                break;
            }
            pre = cur;
            cur = cur.next;
        }
        return dummy.next;
    }
}
```
* 本题中给的`val`是`int`类型，如果给的是`ListNode`，则在删除时不需遍历数组，只删除`val`的第一个节点即可。这样的均摊时间复杂度为$O(1)$。给出以下参考代码：
```java
class Solution {
    public ListNode deleteNode(ListNode head, ListNode val) {
        if(val == head)
            return head.next;
        if(val.next != null){
            ListNode next = val.next;
            val.val = next.val;
            val.next = next.next;
        }
        else{
            ListNode cur = head;
            while(cur.next != null){
                cur = cur.next;
            }
            cur.next = null;
        }
        return head;
    }
}
```
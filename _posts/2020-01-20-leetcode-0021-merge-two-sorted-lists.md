---
layout: post
title: LeetCode 0021 题解
description: "合并两个有序链表"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

### 题解
1. 法一（递归法）
```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1 == null)
            return l2;
        if(l2 == null)
            return l1;
        if(l1.val <= l2.val){
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        }  
        else{
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
    }
}
```
2. 法二（迭代法）
```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode newHead = new ListNode(-1);
        ListNode prev = newHead;
        while(l1 != null && l2!= null){
            if(l1.val <= l2.val){
                prev.next = l1;
                l1 = l1.next;
            }
            else{
                prev.next = l2;
                l2 = l2.next;
            }
            prev = prev.next;
        } 
        prev.next = l1 == null ? l2 : l1;
        return newHead.next;
    }
}
```
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

### 思路
创建新链表以存放最终结果。依次比较两个初始链表的元素大小，将较小的放入新链表，并将新链表与取出元素的链表后移一位；当某一个链表为空时，将另一个链表直接复制到新链表中。

### 题解

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode rel = new ListNode(-1);
        ListNode relhead = rel;
        while(l1 != null && l2 != null){
            if(l1.val < l2.val){
                rel.next = l1;
                l1 = l1.next;
            }
            else {
                rel.next = l2;
                l2 = l2.next;
            }
            rel = rel.next;
        }
        rel.next = (l1 == null) ? l2 : l1;
        return relhead.next;
    }
}
```
* 方法二：递归  

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1 == null) return l2;
        if(l2 == null) return l1;
        if(l1.val < l2.val){
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



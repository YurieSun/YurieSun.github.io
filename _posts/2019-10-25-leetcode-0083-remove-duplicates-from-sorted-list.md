---
layout: post
title: LeetCode 0083 题解
description: "删除排序链表中的重复元素"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

### 思路
若遇到相同元素，则跳过并指向下一元素；若不相同，则移动当前元素。

### 题解
1. 法一（迭代法）
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
    public ListNode deleteDuplicates(ListNode head) {
        ListNode cur = head;
        while(cur != null && cur.next != null) {
        	if(cur.val == cur.next.val) 
        		cur.next = cur.next.next;
        	else
        		cur = cur.next;
        }
        return head;
    }
}
```
2. 法二（递归法）
```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if(head == null || head.next == null)
            return head;
        head.next = deleteDuplicates(head.next);
        return head.val == head.next.val ? head.next : head;
    }
}
```
---
layout: post
title: LeetCode 0445 题解
description: "两数相加 II"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[两数相加 II](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

### 题解
1. 法一（栈）：先将两个链表的值分别存入两个栈中，再出栈相加，并组成链表。
```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        Stack<Integer> s1 = buildStack(l1);
        Stack<Integer> s2 = buildStack(l2);
        ListNode dummy = new ListNode(-1);
        int carry = 0;
        while(!s1.isEmpty() || !s2.isEmpty() || carry != 0){
            int num1 = s1.isEmpty() ? 0 : s1.pop();
            int num2 = s2.isEmpty() ? 0 : s2.pop();
            int num = num1 + num2 + carry;
            ListNode node = new ListNode(num % 10);
            // 出栈是从低位到高位的，因此这里是将当前计算的节点插入到dummy后面。
            node.next = dummy.next;
            dummy.next = node;
            carry = num / 10;
        }
        return dummy.next;
    }
    private Stack<Integer> buildStack(ListNode head){
        Stack<Integer> stack = new Stack<>();
        while(head != null){
            stack.push(head.val);
            head = head.next;
        }

        return stack;
    }
}
```
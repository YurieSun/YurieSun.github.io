---
layout: post
title: 剑指Offer 06 题解
description: "从尾到头打印链表"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

### 题解
1. 法一：先将链表反转，这样头节点正好指向最后一个节点，再遍历反转后的链表，并将数值记录到数组中。反转链表有迭代和递归两种做法。这里采用迭代。
```java
class Solution {
    private int cnt;
    public int[] reversePrint(ListNode head) {
        if(head == null)
            return new int[0];
        ListNode newHead = reverse(head);
        int[] res = new int[cnt];
        int i = 0;
        ListNode cur = newHead;
        while(cur != null){
            res[i++] = cur.val;
            cur = cur.next;
        }   
        return res;
    }
    public ListNode reverse(ListNode head){
        ListNode pre = null, cur = head;
        while(cur != null){
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
            cnt++; 
        }
        return pre;
    }
}
```
* 使用头插法反转链表
```java
class Solution {
    private int cnt;
    public int[] reversePrint(ListNode head) {
        if(head == null)
            return new int[0];
        ListNode dummy = new ListNode(-1);
        while(head != null){
            ListNode next = head.next;
            head.next = dummy.next;
            dummy.next = head;
            head = next;
            cnt++;
        }
        int[] res = new int[cnt];
        for(int i = 0; i < res.length; i++){
            dummy = dummy.next;
            res[i] = dummy.val;
        }
        return res;
    }
}
```
2. 法二（递归）：也可直接使用递归进行操作，此时不需要反转链表。
```java
class Solution {
    private ArrayList<Integer> temp = new ArrayList<>();
    public int[] reversePrint(ListNode head) {
        if(head == null)
            return new int[0];
        reverse(head);
        int[] res = new int[temp.size()];
        for(int i = 0; i < res.length; i++)
            res[i] = temp.get(i);
        return res;
    }
    public void reverse(ListNode head){
        if(head == null)
            return;
        reverse(head.next);
        temp.add(head.val);
    }
}
```
3. 法三（栈）：递归其实是在隐性调用栈，也可直接使用栈。遍历链表时将数值存入栈，再将栈中元素依次存入数组。
```java
class Solution {
    public int[] reversePrint(ListNode head) {
        if(head == null)
            return new int[0];
        Stack<Integer> stack = new Stack<>();
        while(head != null){
            stack.push(head.val);
            head = head.next;
        }
        int[] res = new int[stack.size()];
        for(int i = 0; i < res.length; i++)
            res[i] = stack.pop();
        return res;
    }
}
```
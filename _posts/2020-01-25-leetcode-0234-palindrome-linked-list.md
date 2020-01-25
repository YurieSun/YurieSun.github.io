---
layout: post
title: LeetCode 0234 题解
description: "回文链表"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)

### 题解
1. 法一（栈）：第一次遍历计算链表长度，第二次遍历将前半段元素入栈，然后依次将元素出栈，并与后半段对应的元素比较是否相等。（不满足空间复杂度为 $O(1)$ ）
```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        ListNode cur = head;
        int count = 0;
        Stack<Integer> stack = new Stack<>();
        while(cur != null){
            count++;
            cur = cur.next;
        }
        cur = head;
        int half = count / 2;
        while(half > 0){
            stack.push(cur.val);
            cur = cur.next;
            half--;
        }
        if(count % 2 != 0)
            cur = cur.next;
        while(!stack.isEmpty()){
            if(cur.val != stack.pop())
                return false;
            cur = cur.next;
        }
        return true;
    }
}
```
2. 法二：先用快慢指针找到中间节点，再将链表切分成两段，并将后半段进行反转，随后扫描两段链表检查是否相等。（满足空间复杂度为 $O(1)$ ）
```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        if(head == null || head.next == null)
            return true;
        ListNode cur = head.next;
        ListNode mid = head;
        while(cur != null && cur.next != null){
            mid = mid.next;
            cur = cur.next.next;
        }
        if(cur != null)
            mid = mid.next;
        cut(head, mid);
        return isEqual(head, reverse(mid));
    }
    private void cut(ListNode head, ListNode cutNode){
        while(head.next != cutNode)
            head = head.next;
        head.next = null;
    }
    private ListNode reverse(ListNode head){
        ListNode pre = null;
        while(head != null){
            ListNode next = head.next;
            head.next = pre;
            pre = head;
            head = next;
        }
        return pre;  
    }
    private boolean isEqual(ListNode l1, ListNode l2){
        while(l1 != null && l2 != null){
            if(l1.val != l2.val)
                return false;
            l1 = l1.next;
            l2 = l2.next;
        }
        return true;
    }
}
```
---
layout: post
title: LeetCode 0160 题解
description: "相交链表"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

### 题解
1. 法一（暴力二重循环）：对于第一个链表的每一个结点，遍历第二个链表的节点并检查是否相等。时间复杂度为 $O(mn)$ ，空间复杂度为 $O(1)$。
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA == null || headB == null)
            return null;
        ListNode pA = headA;
        while(pA != null){
            ListNode pB = headB;
            while(pB != null){
                if(pA == pB)
                    return pA;
                pB = pB.next;
            }
            pA = pA.next;
        }
        return null;
    }
}
```
2. 法二（hashset法）：将第一个链表的结点放入`hashset`中，之后扫描第二个链表，检查在`hashset`中是否存在该结点。时间复杂度为 $O(m+n)$ ，空间复杂度为 $O(m)$ 或 $O(n)$ 。
```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA == null || headB == null)
            return null;
        ListNode pA = headA, pB = headB;
        Set<ListNode> set = new HashSet<>();
        while(pA != null){
            set.add(pA);
            pA = pA.next;
        }
        while(pB != null){
            if(set.contains(pB))
                return pB;
            pB = pB.next;
        }
        return null;
    }
}
```
3. 法三（双指针法）：两个指针分别从两个链表起始结点开始，若某一个指针达到末尾，则该指针指向另一链表开头，两指针均要遍历两链表。若链表相交，则当两指针会到达相同结点；若链表不相交，则两指针遍历完毕后会同时到达末尾，则可退出循环并返回`null`。时间复杂度为 $O(n)$ ，空间复杂度为 $O(1)$。
```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA == null || headB == null)
            return null;
        ListNode pA = headA, pB = headB;
        while(pA != pB){
            pA = pA == null ? headB : pA.next;
            pB = pB == null ? headA : pB.next;
        }
        return pA;
    }
}
```
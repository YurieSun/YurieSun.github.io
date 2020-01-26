---
layout: post
title: LeetCode 0725 题解
description: "分隔链表"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[分隔链表](https://leetcode-cn.com/problems/split-linked-list-in-parts/)

### 题解
1. 法一：若要满足题目条件，则先将节点平均分配到数组中，多出来的节点从头开始多分一个，由此可确定数组中每个链表的长度。
```java
class Solution {
    public ListNode[] splitListToParts(ListNode root, int k) {
        ListNode[] ans = new ListNode[k];
        ListNode cur = root;
        int count = 0;
        while(cur != null){
            count++;
            cur = cur.next;
        }
        int size = count / k;
        int extra = count % k;
        int i = 0;
        cur = root;
        while(cur != null && i < k){
            int curSize = size + (extra-- > 0 ? 1 : 0);
            ans[i] = cur;
            while(curSize - 1 > 0){
                cur = cur.next;
                curSize--;
            }
            ListNode next = cur.next;
            cur.next = null;
            cur = next;
            i++;
        }
        return ans;
    }
}
```
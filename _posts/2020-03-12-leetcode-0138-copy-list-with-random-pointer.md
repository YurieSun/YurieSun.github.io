---
layout: post
title: LeetCode 0138 题解
description: "复制带随机指针的链表"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[复制带随机指针的链表](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)

### 题解
1. 法一（哈希表+迭代）：遍历链表，并创建对应的新节点，将原节点和新节点放入哈希表。再次遍历链表，将新节点的`next`和`random`通过哈希表查出并连接。
```java
class Solution {
    public Node copyRandomList(Node head) {
        if (head == null)
            return null;
        Map<Node, Node> map = new HashMap<>();
        Node cur = head;
        while (cur != null) {
            Node clone = new Node(cur.val);
            map.put(cur, clone);
            cur = cur.next;
        }
        cur = head;
        while (cur != null) {
            Node clone = map.get(cur);
            clone.next = map.get(cur.next);
            clone.random = map.get(cur.random);
            cur = cur.next;
        }
        return map.get(head);
    }
}
```
2. 法二（哈希表+递归）
```java
class Solution {
    Map<Node, Node> map = new HashMap<>();
    public Node copyRandomList(Node head) {
        if (head == null)
            return null;
        if (map.containsKey(head))
            return map.get(head);
        Node clone = new Node(head.val);
        map.put(head, clone);
        clone.next = copyRandomList(head.next);
        clone.random = copyRandomList(head.random);
        return clone;
    }
}
```
3. 法三（原地复制）：遍历链表，对于每个节点，都创建一个对应的新节点，并把新节点插入原节点后面。遍历新链表，并通过原节点的`random`连接建立新节点的`random`连接。最后，将新旧节点的连接拆开，可以得到复制后的链表。
```java
class Solution {
    public Node copyRandomList(Node head) {
        if (head == null)
            return null;
        // 将新节点插入原节点后面
        Node cur = head;
        while (cur != null) {
            Node clone = new Node(cur.val);
            clone.next = cur.next;
            cur.next = clone;
            cur = clone.next;
        }
        // 建立新节点的random连接
        cur = head;
        while (cur != null) {
            Node clone = cur.next;
            if (cur.random != null)
                clone.random = cur.random.next;
            cur = clone.next;
        }
        // 拆分新旧节点
        cur = head;
        Node newHead = head.next;
        while (cur.next != null) {
            Node next = cur.next;
            cur.next = next.next;
            cur = next;
        }
        return newHead;
    }
}
```
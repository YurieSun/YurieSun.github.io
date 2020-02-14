---
layout: post
title: LeetCode 0225 题解
description: "用队列实现栈"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[用队列实现栈](https://leetcode-cn.com/problems/implement-stack-using-queues/)

### 题解
1. 法一：用一个队列即可实现。当入栈时，先将元素`x`入队，再将除了`x`之外的元素重新入队；当出栈时，直接将元素出队。

```java
class MyStack {
    private Queue<Integer> queue;
    /** Initialize your data structure here. */
    public MyStack() {
        queue = new LinkedList<>();
    }
    
    /** Push element x onto stack. */
    public void push(int x) {
        queue.add(x);
        int cnt = queue.size();
        while(cnt-- > 1)
            queue.add(queue.poll());
    }
    
    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        return queue.poll();
    }
    
    /** Get the top element. */
    public int top() {
        return queue.peek();
    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        return queue.isEmpty();
    }
}

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * boolean param_4 = obj.empty();
 */
```
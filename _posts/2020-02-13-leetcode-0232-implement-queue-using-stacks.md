---
layout: post
title: LeetCode 0232 题解
description: "用栈实现队列"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[用栈实现队列](https://leetcode-cn.com/problems/implement-queue-using-stacks/)

### 题解
1. 法一：通过两个栈实现。当入队时，若`out`不为空，则先将其中的元素放入`in`中，再进行入栈操作；当出队时，若`in`不为空，则先将其中的元素放入`out`中，再进行出栈操作。

```java
class MyQueue {
    Stack<Integer> in;
    Stack<Integer> out;
    /** Initialize your data structure here. */
    public MyQueue() {
        in = new Stack<>();
        out = new Stack<>();
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        while(!out.isEmpty()){
            in.push(out.pop());
        }
        in.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        while(!in.isEmpty())
            out.push(in.pop());
        return out.pop();
    }
    
    /** Get the front element. */
    public int peek() {
        while(!in.isEmpty())
            out.push(in.pop());
        return out.peek();
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return in.isEmpty() && out.isEmpty();
    }
}
```
* 改进：栈`in`看成队列入口，栈`out`看成队列出口。入队时，直接对`in`入栈；出队时，若`out`不为空，则直接出栈，若为空，则将`in`中的元素放入`out`中，再出栈。

```java
class MyQueue {
    Stack<Integer> in;
    Stack<Integer> out;
    /** Initialize your data structure here. */
    public MyQueue() {
        in = new Stack<>();
        out = new Stack<>();
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        in.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        in2out();
        return out.pop();
    }
    
    /** Get the front element. */
    public int peek() {
        in2out();
        return out.peek();
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return in.isEmpty() && out.isEmpty();
    }
    public void in2out(){
        if(out.isEmpty()){
            while(!in.isEmpty())
            out.push(in.pop());
        }   
    }
}
```
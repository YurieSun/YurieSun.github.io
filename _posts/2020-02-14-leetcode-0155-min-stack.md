---
layout: post
title: LeetCode 0155 题解
description: "最小栈"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[最小栈](https://leetcode-cn.com/problems/min-stack/)

### 题解
1. 法一：用两个栈实现，两个栈的出栈和入栈是同步的。数据栈存放数据，和普通栈操作一样；最小栈和数据栈同步出栈、入栈，每个栈顶元素都是当前所有元素的最小值。

```java
class MinStack {
    private Stack<Integer> dataStack;
    private Stack<Integer> minStack;
    private int min;
    /** initialize your data structure here. */
    public MinStack() {
        dataStack = new Stack<>();
        minStack = new Stack<>();
        min = Integer.MAX_VALUE;
    }
    
    public void push(int x) {
        dataStack.push(x);
        min = Math.min(min, x);
        minStack.push(min);
    }
    
    public void pop() {
        dataStack.pop();
        minStack.pop();
        min = minStack.isEmpty() ? Integer.MAX_VALUE : minStack.peek();
    }
    
    public int top() {
        return dataStack.peek();
    }
    
    public int getMin() {
        return minStack.peek();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```

* 以上方法的最小栈所用空间与数据栈相同，较浪费空间。因此可以采用两个栈实现，且两个栈的出栈和入栈是不同步的。数据栈存放数据，和普通栈操作一样；对于最小栈，只有最小栈为空或入栈的值小于等于最小栈的栈顶元素时才进行入栈操作，只有出栈的值与最小栈的栈顶元素相等时才出栈。

```java
class MinStack {
    private Deque<Integer> data;
    private Deque<Integer> min;

    public MinStack() {
        data = new LinkedList<>();
        min = new LinkedList<>();
    }

    public void push(int x) {
        data.push(x);
        if (min.isEmpty() || x <= min.peek()) {
            min.push(x);
        }
    }

    public void pop() {
        /**
         * 注意：原来的写法 if(data.pop() == min.peek())进行最小值栈是否需要pop的判断有问题。
         * 这是因为Deque中存放的是Integer，若直接出栈进行比较，比较的是对象而不是值，
         * 因此会出现值相等而引用不同造成的if判断错误。所以先出栈变为int，此时才是判断值是否相等。
         *
         **/
        int res = data.pop();
        if (res == min.peek()) {
            min.pop();
        }
    }

    public int top() {
        return data.peek();
    }

    public int getMin() {
        return min.peek();
    }
}
```

2. 法二：使用一个栈实现。入栈时，若当前元素`x`比最小值`min`小，则先将`min`入栈，再将`x`入栈，同时更新最小值；出栈时，若出栈元素`x`等于当前最小值`min`，则先将`x`出栈，再出栈一个元素来赋值给`min`。

```java
class MinStack {
    private Stack<Integer> stack;
    private int min;
    /** initialize your data structure here. */
    public MinStack() {
        stack = new Stack<>();
        min = Integer.MAX_VALUE;
    }
    
    public void push(int x) {
        if(x <= min){
            stack.push(min);
            min = x;
        }
        stack.push(x);
    }
    
    public void pop() {
        if(stack.pop() == min)
            min = stack.pop();
    }
    
    public int top() {
        return stack.peek();
    }
    
    public int getMin() {
        return min;
    }
}
```
* 另一个采用一个栈实现的思路：用`min`保存最小值，当入栈时，将`x-min`入栈，若`x`大于等于`min`，则入栈元素为非负数，若`x`小于`min`，则入栈元素为负数；当出栈时，若栈顶元素为非负数，则可用出栈元素+`min`进行恢复，若栈顶元素为负数，说明存入时最小值被更新过，则用`min`减栈顶元素恢复最小值，将该值更新至`min`。缺点是用到减法，`int`可能会溢出，因此改成了`long`类型。

```java
class MinStack {
    private Stack<Long> stack;
    private long min;
    /** initialize your data structure here. */
    public MinStack() {
        stack = new Stack<>();
        min = Long.MAX_VALUE;
    }
    
    public void push(int x) {
        if(stack.isEmpty()){
            min = x;
            stack.push(x - min);
        }
        else{
            stack.push(x - min); 
            min = Math.min(min, x);
        }
    }
    
    public void pop() {
        long num = stack.pop();
        if(num < 0)
            min -= num;
    }
    
    public int top() {
        long num = stack.peek();
        if(num < 0)
            return (int)min;
        return (int)(num + min);
    }
    
    public int getMin() {
        return (int)min;
    }
}
```

3. 法三：不采用栈而采用链表实现，需在节点中增加一个最小值的属性。入栈时，将节点插入头部；出栈则直接抛弃头结点。

```java
class MinStack {
    class Node{
        int num;
        int min;
        Node next;
        Node(int num, int min){
            this.num = num;
            this.min = min;
        } 
    }
    Node head;
    /** initialize your data structure here. */
    public MinStack() {
        
    }
    
    public void push(int x) {
        if(head == null)
            head = new Node(x, x);
        else{
            //把新节点插到头部
            Node node = new Node(x, Math.min(x, head.min));
            node.next = head;
            head = node;
        }
               
    }
    
    public void pop() {
        head = head.next;
    }
    
    public int top() {
        return head.num;
    }
    
    public int getMin() {
        return head.min;
    }
}
```
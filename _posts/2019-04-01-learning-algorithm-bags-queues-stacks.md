---
layout: post
title: 算法笔记（三)
description: "学习资料：algorithm (Princeton University)"
keywords: test
category: Learning
tags: [algorithm]
---


## 包、队列和栈

### Introduction
1. 基本数据类型  
* 值：对象集； 
* 操作：插、删、迭代、检验是否为空；
* 栈：last in first out (LIFO)  
队列：first in first out (FIFO)  
2. client, implementation, interface
* client：使用在接口中已定义好的操作的程序；  
implementation：真正实现操作的代码；  
interface：数据类型、基本操作的描述；
3. 将接口与实现分离的好处
* 客户端无需知道实现的细节，同时也有很多实现方法进行选择；
* 实现无需知道客户端的需求细节，则实现可供不同的客户端重复使用；
* 创建模块、可重用的数据库；
* 在重要的地方使用优化的实现，以提高性能。

### Stacks
1. Stack API
```java
public class StackOfStrings
{
    StackOfStrings();//创建空栈
    void push(String item);//push操作
    String pop();//pop操作
    boolean isEmpty();//判断栈是否为空
    int size();//可选，统计栈中字符个数
} 
```
2. Stack test client
* 功能：从标准输入中读字符，若字符为“-”，从栈中取出字符并打印，其余情况均将字符放入栈中。
* java实现
```java
public static void main(String[] args)
{
    StackOfStrings stack = new StackOfStrings();
    while (!StdIn.isEmpty())
    {
        if (s.equals("-"))
            StdOut.print(stack.pop());
        else
            stack.push(s);
    }
} 
```
3. Stack implementation
* 链表实现

```java
public class LinkedStackOfStrings()
{
    private Node first = null;

    private class Node
    {
        String item;
        Node next;
    }

    public boolean isEmpty()
    {
        return first == null;
    }

    public void push(String item)
    {
        Node oldfirst = first;
        first = new Node()
        Node.item = item;
        Node.next = oldfirst;
    }

    public String pop()
    {
        String item = first.item;
        first = first.next;
        return item;
    }
}
```
* 数组实现
```java
public class FixedCapacityStackOfStrings()
{
    private String[] s;
    private int N = 0;

    public FixedCapacityStackOfStrings(int capacity)//缺陷：需提前给定数组大小，会有溢出问题；且需要client提供容量，破坏了API
    {
        s = new String[capacity];
    }

    public boolean isEmpty()
    {
        return N == 0;
    }

    public void push(String item)
    {
        s[N++] == item;
    }

    public String pop()
    {
        return s[--N];
    }
}
```
3. Stack considerations
* 不足与溢出
不足：当栈为空时执行pop操作；
溢出：当栈满时执行push操作。
* 以上实现允许空对象的插入
* 游离
当执行pop操作时，该位置还占有内存，因此，将pop操作改为：
```java
public String pop()
{
    String item = s[--N];
    s[N] = null;
    return item;
}
```

### Resizing Arrays

### Queues

### Generics

### Iterations

### Applications
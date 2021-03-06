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
    * implementation：真正实现操作的代码；  
    * interface：数据类型、基本操作的描述；
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

        public FixedCapacityStackOfStrings(int capacity)
        //缺陷：需提前给定数组大小，会有溢出问题；且需要client提供容量，破坏了API。
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
用数组实现栈时产生了需要客户端提供容量的问题，因此需要使数组长度自动伸缩，以解决该问题。
1. 方法一
    * 每执行一次push()操作时将数组长度增加1，没执行一次pop()操作将数组长度减小1。
    * 花销太大：每次操作需将数组复制到新数组中，则若插入N个对象，时间将正比于$1+2+...+N$~$\frac{1}{2}N^2。
    * 因此需减少数组改变长度的频率。 
2. 方法二
    * 数组满时，创建一个长度为原来的两倍的新数组，并复制对象。

    ```java
    public ResizingArraysStackOfStrings()
    {
        s = new String[1];
    }

    public void push(String item)
    {
        if (N == s.length)
            resize(2 * s.length);
        s[N++] = item;
    }

    public void resize(int capacity)
    {
        String[] copy = new String[capacity];
        for (int i = 0; i < N; i++)
            copy[i] = s[i];
        s = copy;
    }
    ```

    * 花销：插入N个对象的时间与N成正比，因仅在2的倍数时进行复制数组的操作，则平摊下来所需时间与N成正比。（平摊是一个有效策略）
    * 在数组$\frac{1}{4}$满时，将数组长度缩减为原来的一半，并复制对象。
    * 为什么不在数组$\frac{1}{2}$满时缩减数组长度？  
    在最坏情况下，当数组满时若不断交替执行push、pop操作，则每次操作均要改变数组长度，花销太大。（抖动问题）

    ```java
    public String pop()
    {
        String item = s[--N];
        s[N] = null;
        if (N > 0 && N == s.length/4)
            resize(s.length/2);
        return item;
    }
    ```

3. 栈实现的对比：链表与可调整数组
    * 链表：在最坏情况下每个操作所需时间为常数，但需要额外的时间与空间来处理链表；
    * 可调整数组：每个操作平摊分析下所需时间为常数，但不需要更多的空间；
    * 因此，若想保证每个操作都快速完成，选择链表；若想使用更少的总时间与空间，选择数组。

### Queues
1. Queue API  
```java
public class QueueOfSreings
{
    QueueOfStrings();//创建空队列
    void enqueue(Sttring item);//入队
    String dequeue();//出队
    boolean isEmpty();//检验是否为空
    int size();
}
```
2. Queue implementation

* 链表实现

```java
public class LinkedQueueOfStrings
{
    private Node first, last;

    public class node
    {
        String item;
        Node next
    }

    public boolean isEmpty()
    {
        return first == null;
    }

    public void enqueue(String item)
    {
        Node oldlast = last;
        last.item = item;
        last.next = null;
        if (isEmpty())
            first = last;
        else
            oldlast.next = last;
    }

    public String dequeue()
    {
        String item = first.item;
        first = first.next;
        if (isEmpty())
            last = null;
        return item;
    }
}
```

* 数组实现

```java
public class ResizingArraysQueueOfStrings
{
    private String[] q;
    private int N = 0, head = 0, tail = 0;
    //N指数组内字符的实际长度，length属性得到的是数组的声明长度。

    public ResizingArraysQueueOfStrings()
    {
        s = new String[1];
    }

    public boolean isEmpty()
    {
        return N == 0;
    }

    public void enqueue(String)
    {
        if(N == q.length)
            resize(2 * q.length);
        q[tail++] = item;
        N++;
    }

    public String dequeue()
    {
        String item = q[head];
        q[head++] = null;
        N--;
        if (N >0 && N == q.length/4)
            resize (q.length/2);
        return item
    }

    private void resize(int capacity)
    {
        String[] copy = new String[capacity];
        for (int i = 0; i < N; i++)
            copy[i] = q[i];
        q = copy;
    }
}
```

### Generics

泛型是解决代码重用在不同数据类型的方法，可避免强制类型转换，且在编译时发现类型不匹配的问题而非运行时。

链表的泛型实现如下：

```java
public class Stack<Item>
{
    private Node first = null;

    private class Node
    {
        Item item;
        Node next;
    }


    public boolean isEmpty()
    {
        return first == null;
    }

    public void push(Item item)
    {
        Node oldfirst = first;
        first = new Node();
        first.item = item;
        first.next = oldfirst;
    }

    public Item pop()
    {
        Item item = first.item;
        first = first.next;
        return item;
    }
}
```

在java中不允许创建泛型数组，因此无法用数组实现泛型，则可以使用下面的强制类型转换，但不是一个好方法。该方法在编译时会产生warning，但不需要管，这是java不允许创建泛函数组所导致的。

```java
public class FixedCapacityStack<Item>
{
    private Item[] s;
    private int N = 0;

    public FixedCapacityStack(int capacity)
    {
        s = (Item[]) new Object[capacity];
    }

    public boolean isEmpty()
    {
        return N == 0;
    }

    public void push(Item item)
    {
        s[N++] = item;
    }

    public item pop()
    {
        item = s[--N];
    }
}
```


### Iterations

* Iterable interface //返回Iterator的方法

```java
public interface Iterable<Item>
{
    Iterator<Item> iterator();
}
```

* Iterator interface 

```java
public interface Iterator<Item>
{
    boolean hasNext();
    Item next();
    void remove(); //一般不用，存在调试隐患。
}
```

* 有迭代器时，“foreach”代码如下：

```java
for (String s : stack)
    StdOut.println(s);
```

* 无迭代器时，等价代码如下：

```java
Iterator<String> i = stack.iterator();
while (i.hasNext())
{
    String s = i.next();
    StdOut.println(s);
}
```

* Stack iterator

```java
//链表实现
import java.util.Iterator;

public class Stack<Item> implements Iterable<Item>
{
    ...

    public Iterator<Item> iterator()
    {
        return new ListIterator();
    }

    private class ListIterator implements Iterator<Item>
    {
        private Node current = first;

        public boolean hasNext() {return current != null;}
        public void remove() {// not supported}
        public Item next()
        {
            Item item = current.item;
            current = current.next;
            return item;
        }
    }
}

//数组实现
import java.util.Iterator;

public class Stack<Item> implements Iterable<Item>
{
    ...

    public Iterator<Item> iterator()
    {
        return new ReverseArrayIterator();
    }

    private class ReverseArrayIterator implements Iterator<Item>
    {
        private int i = N;

        public boolean hasNext() {return i > 0;}
        public void remove {// not supported}
        public Item next() {return s[--i];}
    }
}
```
* Bag API

这种数据结构只需插入元素，并不关心顺序。

```java
public class Bag<Item> implements Iterable<Item>

    Bag() //创建空包
    void add(Item x) //插入新元素
    int size() //包中的元素个数
    Iterable<Item> iterator() //包中元素的迭代
```

### Applications

* java的库中有实现栈、队列等数据结构的接口，但不推荐使用，因为里面有许多我们不需要用到的操作，且目前对库的理解不深，会导致错误及性能下降。

* 栈的应用：java虚拟机、打字的撤销操作、网页的后退功能、函数调用

* 栈的典型应用：中缀表达式

```java
public class Evaluate
{
    public static void main(String[] args)
    {
        Stack<String> ops = new Stack<String>();
        Stack<Double> vals = new Stack<Double>();
        while (!StdIn.isEmpty())
        {
            String s = StdIn.readString();
            if (s.equals("("))  ;
            else if (s.equals("+")) ops.push(s);
            else if (s.equals("-")) ops.push(s);
            else if (s.equals(")"))
            {
                String op = ops.pop();
                if (op.equals("+")) vals.push(vals.pop() + vals.pop());
                else if (op.equals("*")) vals.push(vals.pop() * vals.pop());
            }
            else vals.push(Double.parseDouble(s));
        }
        StdOut.println(vals.pop());
    }
}
```
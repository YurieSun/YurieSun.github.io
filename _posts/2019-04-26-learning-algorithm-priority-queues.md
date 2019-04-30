---
layout: post
title: 算法笔记（七）
description: "学习资料：algorithm (Princeton University)"
keyword: test
catogery: learning
tag: [algorithm]
---

## Priority Queues
1. 不同数据结构的区别    
    Stack：删除最近加入的元素  
    Queue：删除第一个加入的元素  
    Randomized queue：随机删除  
    Priority Queue：删除最大（或最小）的元素
2. priority queue API
    ```java
    public class MaxPQ<Key extends Comparable<Key>> //元素必须是Comparable类型
    MaxPQ(); //创建新的优先队列
    MaxPQ(Key[] a) //可选，创建带有元素的优先队列
    void insert(Key v) //插入元素
    Key delMax() //删除并返回最大元素
    boolean isEmpty() //查看是否为空
    Key max() //可选，返回最大元素
    int size() //可选，返回元素个数
    ```
3. priority queue的应用  
    * Event-driven simulation: customers in a line, colliding particles  
    * Numerical computation: reducing roundoff error
    * Data compression: Huffman codes
    * Graph searching: Dijkstra's algorithm, Prim's algorithm
    * Number theory: sum of powers
    * Artificial intelligence: A* search
    * Statistics: maintain largest M values in a sequence
    * Operating systems: load balancing, interrupt handling
    * Discrete optimization: big packing, scheduling
    * Spam filtering: Bayesian spam filter
4. priority queue client举例  
    * 寻找$N$个元素中最大的$M$个，通常用于内存不足以存放$N$，仅能存放$M$个元素的情况。如在检测诈骗时，银行的转账记录仅保存转账金额最大的几笔。

    ```java
    MinPQ<Transaction> pq = new MinPQ<Transaction>();

    while (StdIn.hasNextLine())
    {
        String line = StdIn.readline();
        Transaction item = new Transaction(line);
        pq.insert(item);
        if (pq.size() > M)
            pq.delMin();
    } //若容量已满，则删去最小元素
    ```
    * 不同方法所使用的时间与空间  

    |implementation|time|space|
    |:--:|:-:|:-:|
    |sort|$N\log N$|$N$|
    |elementary PQ|$M N$|$M$|
    |binary heap|$N\log N$|$M$|
    |best in theory|$N$|$M$|
5. 不排序的数组实现
    ```java
    public class UnorderedMaxPQ<Key extends Comparable<Key>>
    {
        private Key[] pq;
        private int N;

        public UnorderedMaxPQ(int capacity)
        {
            pq = (Key[]) new Comparable[capacity];
            //需要客户端提供容量，可以通过resizing array解决
        }

        public boolean isEmpty()
        {
            return N == 0;
        }

        public void insert(Key x)
        {
            pq[N++] = x;
        }

        public Key delMax()
        {
            int max = 0;
            for (int i = 1; i < N; i++)
                if (less(max, i)) max = i;
            exch(max, N-1);
            return pq[--N];
        }
    }
    ```
    * 排序与不排序的区别
    
    |implementation|insert|del max|max|
    |:-:|:-:|:-:|:-:|
    |unordered array|$1$|$N$|$N$|
    |ordered array|$N$|$1$|$1$|
    |goal|$\log N$|$\log N$|$\log N$|

### API and Elementqry Implementations
1. 完全二叉树  
    除最后一层外其余节点都是满的二叉树，称为完全二叉树。  
    性质：具有$N$个节点的完全二叉树的高度为$\lfloor\lg N\rfloor$，因为只有$N$为2的指数幂时，高度才会增加。
2. 二叉堆的表示与性质
    * 表示：用数组表示二叉堆，这样，仅需操作数组下标，即可完成二叉堆的操作。其中，已排序的二叉堆中任意父节点都不会比子节点小，而数组从下标为$1$开始储存元素，即$a[0]$为空。
    * 性质：用以上方式表示的二叉堆，最大元素为$a[1]$，节点$k$的父节点为$\frac{k}{2}$，子节点为$2k$和$2k+1$。
3. 二叉堆的基本操作
    * swim操作  
    ```java
    //把比父节点大的子节点上移至正确位置
    private void swim(int k)
    {
        while (k > 1 && less(k, k/2))
        {
            exch(k, k/2);
            k = k/2;
        }
    }
    ```
    * insert操作
    ```java
    //插入元素
    private void insert(Key x)
    {
        pq[++N] = x;
        swim(N);
    }
    ```
    * sink操作
    ```java
    //把比子节点小的父节点下移至正确位置
    private void sink(int k)
    {
        while (2*k <= N)
        {
            int j = 2*k;
            if (less(j, j+1)) j++;//找到两个子节点中较大的进行交换
            if (!less(k, j)) break;
            exch(k, j)
            k = j;
        }
    }
    ```
    * 删除最大元素  
    最大比较次数为$2\lg N$。
    ```java
    //将根节点与最后一个节点交换位置，再对新的根节点做sink操作。
    public Key delMax()
    {
        Key = pq[1];
        exch(1, N--);
        sink(1);
        pq[N+1] = null;
        return max;
    }
    ```
4. 二叉堆的java实现
    ```java
    public class MaxPQ<Key extends Comparable<Key>>
    {
        private Key[] pq;
        private int N;
        //可用resizing array避免固定数组长度
        public MaxPQ(int capacity)
        { pq = (Key[]) new Comparable[capacity+1]; }
        //pq的操作
        public boolean isEmpty()
        { return N == 0; }
        public void insert(Key key)
        { /* as before */ }
        public Key delMax()
        { /* as before */ }
        //二叉堆辅助函数
        private void swim(int k)
        private void sink(int k)
        { /* as before */ }
        //数组辅助函数
        private boolean less(int i, int j)
        { return pq[i].compareTo(pq[j]) < 0; }
        private void exch(int i, int j)
        { Key t = pq[i]; pq[i] = pq[j]; pq[j] = t;}
    }
    ```

5. 优先队列不同实现的开销

    |implementation|insert|del max|max|
    |:-:|:-:|:-:|:-:|
    |unordered array|$1$|$N$|$N$|
    |ordered array|$N$|$1$|$1$|
    |binary heap|$log N$|$\log N$|$1$|
    |d-ary heap|$\log_dN$|$d\log_dN$|$1$|
    |Fibonacci|$1$|$\log N$|$1$|
    |impossible|$1$|$1$|$1$|

6. 二叉堆的一些考虑
    * 元素的不变性（immutability）  
    在上面的实现中，假定client不会改变已在优先队列中的元素。这并不是一个好的方法，最好是使用具有不变性的元素。
    * 下溢与溢出  
    若对空的优先队列进行删除操作会导致异常；对于溢出的问题，使用resizing array可解决。
    * MinPQ的实现  
    将less()替换为greater()，并实现greater()。
    * 其余操作  
    如删除任意元素、改变一个元素的优先级（可通过sink()和swim()实现）。
7. java中的immutability
    * 不可变的数据类型是指一旦该数据的值被指定后不能改变。
    * 不可变数据类型：String, Integer, Double, Color, Vector, Transaction, Point2D  
    可变数据类型：StringBuilder, Stack, Counter, Java array
    * 优点：简化调试过程、存在恶意代码时更安全、简化同时编程过程、在优先队列或符号表中使用这种元素更安全。
    * 缺点：对于每个数据类型值必须创建新的对象。（瑕不掩瑜）

### Binary Heaps

### Heapsort

### Event-Driven simulation


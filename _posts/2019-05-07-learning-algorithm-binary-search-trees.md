---
layout: post
title: 算法笔记（九）
description: "学习资料：algorithm (Princeton University)"
keywords: test
category: Learning
tags: [algorithm]
---

## Binary Search Trees

### BSTs
1. 二叉搜索树
    * 定义  
    BST是具有对称顺序的二叉树。二叉树要么为空，要么有两个互不相连的子树（左、右子树）；对称顺序是指每个结点有一个键值，且该键值满足不大于左子树的所有键值，不小于右子树的所有键值。
    * 基本操作  
    search：若该值小，到左子树找，若该值大，到右子树找，若相等，则找到。  
    insert：若该值小，到左子树找，若该值大，到右子树找，若直到最下面的结点仍未找到，则插入。
2. Java中BST的实现
    ```java
    public class BST<Key extends Comparable<Key>, Value>
    {
        private Node root;
        //BST是对根节点的引用。每个Node有四个字段：Key、Value、对左右子树的引用。
        private class Node
        {
            private Key key;
            private Value val;
            private Node left, right;

            public Node(Key key, Value val)
            {
                this.key = key;
                this.val = val;
            }
        }
        //insert有两种情况：若已有该键，则重置对应的值；若没有，则插入。采用递归的方式，简洁但较难理解。比较次数为1+节点深度。
        public void put(Key key, Value val)
        {
            root = put(root, key, val);
        }
        private Node put(Node x, Key key, Value val)
        {
            if (x == null) return new Node(key, val);
            int cmp = key.compareTo(x.key);
            if (cmp < 0)
                x.left = put(x.left, key, val);
            else if (cmp > 0)
                x.right = put(x.right, key, val);
            else
                x.val = val;
            return x;
        }
        //get返回给定键的值，若没有返回null；比较次数为1+节点深度
        public Value get(Key key)
        {
            Node x = root;
            while (x != null)
            {
                int cmp = key.compareTo(x.key);
                if (cmp < 0) x = x.left;
                else if (cmp > 0) x = x.right;
                else return x.val;
            }
            return null;
        }

        public void delete(Key key)
        { /* see next part */ }

        public Iterable<Key> iterator()
        { /* see next part */}
    }
    ```
3. 树的形状
    * 相同的键对应很多不同的BST，这取决于元素进入BST的顺序。
    * 比较次数均为$1+$节点深度。
    * 最好情况为平衡的树，通常情况下为部分平衡，最坏情况产生于元素完全有序的时候，此时树仅有左子树或右子树，与链表没有区别。
4. 数学分析
    * 若将$N$个不同的元素随机插入BST中，search/insert的比较次数应为$\sim 2\ln N$。这是由于当数列中没有相同元素时，BST与quicksort是一一对应的。
    * 若将$N$个不同的元素随机插入BST中，树的高度应为$\sim 4.311\ln N$。然而，最坏情况下，树的高度为$N$。
5. ST实现的总结
    <table>
    <tr>
        <td rowspan="2">ST implementation</td>
        <td colspan="2">worst-case cost<br>(after N inserts)</td>
        <td colspan="2">average case<br>(after N random inserts)</td>
        <td rowspan="2">ordered iteration?</td>
        <td rowspan="2">key interface</td>
    </tr>
    <tr>
        <td>search</td>
        <td>insert</td>
        <td>search hit</td>
        <td>insert</td>
    </tr>
    <tr>
        <td>sequential search<br>(unordered list)</td>
        <td>N</td>
        <td>N</td>
        <td>N/2</td>
        <td>N</td>
        <td>no</td>
        <td>equals()</td>
    </tr>
    <tr>
        <td>binary search<br>(ordered array)</td>
        <td>lg N</td>
        <td>N</td>
        <td>lg N</td>
        <td>N/2</td>
        <td>yes</td>
        <td>compareTo()</td>
    </tr>
    <tr>
        <td>BST</td>
        <td>N</td>
        <td>N</td>
        <td>1.39 lg N</td>
        <td>1.39 lg N</td>
        <td>compareTo()</td>
    </tr>
    </table>

    * 为什么不通过随机洗牌来保证search/insert在最坏情况下为$4.311\ln N$？


### Ordered operations
1. min/max
    * min：找到最小元素  
    max：找到最大元素  
    从根节点往左，直到碰到值为null的键，则为最小；从根节点往右，直到碰到值为null的键，则为最大。
2. floor/ceiling
    * floor：找到小于等于给定键$k$的最大元素  
    ceiling：找到大于等于给定键$k$的最小元素  
    * 以floor为例，存在三种情况：  
    case 1：$k$与节点上的值相等，则$floor(k)=k$;  
    case 2：$k$比节点上的值小，则$floor(k)$在左子树；  
    case 3：$k$比节点上的值大，则4floor(k)$在右子树。
    * java实现（采用递归）
    ```java
    public Key floor(Key key)
    {
        Node x = floor(root, key);
        if (x == null) return null;
        return x.key;
    }
    private Node floor(Node x, Key key)
    {
        if (x == null) return null;
        int cmp = key.compareTo(x.key);
        if (cmp == 0) return x;
        if (cmp < 0) return floor(x.left, key);
        Node t = floor(x.right, key);
        if (t != null) return t;
        else return x;
    }
    ```
    * ceiling同理
3. rank/select
    * rank：找到小于键$k$点的个数  
    select：找到第$k$大的键
    * 方法：给每个节点增加一个字段，用来存储以该节点为根的子树中的节点数（包括自己）；同时实现size()，来返回根节点的节点数。
    * 计算节点数的java实现
    ```java
    private class Node
    {
        private Key key;
        private Value val;
        private Node left;
        private Node right;
        private int count; //增加节点计数的字段
    }
    public size()
    { return size(root); } //返回根节点size
    private int size(Node x)
    {
        if (x == null) return 0;
        return x.count;
    }
    private Node put(Node x, Key key, Value val)
    {
        if (x == null) return new Node(key, val ,1);
        int cmp = key.compareTo(x.key);
        if (cmp < 0) x.left = put(x.left, key, val);
        else if (cmp > 0) x.right = put(x.right, key, val);
        else x.val = val;
        int count = 1 + size(x.left) + size(x.right);
        return x;
    }
    ```
    * rank的java实现
    ```java
    public int rank(Key key)
    { return rank(key, root); }
    private int rank(Key key,Node x)
    {
        if (x == null) return 0;
        int cmp = key.compareTo(x.key);
        if (cmp < 0) return rank(x.left);
        else if (cmp > 0) return 1 + size(x.left) + rank(x.right);
        else return size(x.left);
    }
    ```
4. BST的顺序遍历
    ```java
    public Iterable<Key> keys()
    {
        Queue<Key> q = new Queue<Key>();
        inorder(root, q);
        return q;
    }
    private void inorder(Node x, Queue<Key q)
    {
        if (x == null) return;
        inorder(x.left, q);
        q.enqueue(x.key);
        inorder(x.right, q);
    }
    ```
5. 有序符号表的操作总结

||sequential search|binary search|BST|
|:-:|:-:|:-:|:-:|
|search|$N$|$\lg N$|$h$|
|insert|$N$|$N$|$h$|
|min/max|$N$|$1$|$h$|
|floor/ceiling|$N$|$\lg N$|$h$|
|rank|$N$|$\lg N$|$h$|
|select|$N$|$1$|$h$|
|ordered iteration|$N\lg N$|$N$|$N$|

* 当元素时随机插入时，BST的高度$h$与$\log N$成正比。

### Deletion
1. 
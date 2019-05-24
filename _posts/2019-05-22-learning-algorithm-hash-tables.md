---
layout: post
title: 算法笔记（十二）
description: "学习资料：algorithm (Princeton University)"
keywords: test
category: Learning
tags: [algorithm]
---

## Hash Tables
### Hash Functions
1. hash的基本思想
    * 将值储存在以键作为索引的数组中（其中，索引是键的函数）。
    * 哈希函数：将键计算成数组索引值的一个方法
    * 讨论的问题
        * 如何计算哈希函数
        * 相等测试：如何检测两个键相等的方法
        * 冲突解决：解决两个键经过哈希函数计算后得到相同的键的算法与数据结构
    * hash是一个经典的space-time tradeoff问题
        * 若没有空间限制：可使用以键作为索引的简单哈希函数，这样不会有冲突问题，但使用空间大，与数组并无区别。
        * 若没有时间限制：可将所有键通过哈希函数变成相同索引，对于冲突问题，采用顺序搜索解决，这与链表并无区别。
        * 时间与空间均有限制：hashing
2. 哈希函数的计算
    * 理想的哈希函数计算效率高，可将键较均匀的产生不同的索引。
    * 问题：对于不同的键类型，需要采用不同的方法。
3. Java中的hash code
    * 所有Java的类都能继承hashCode()方法，该方法返回一个32位整数。
    * 必须有的属性：若x.equals(y)，则(x.hashCode() == y.hashCode())。  
    理想状态下的属性：若!x.equals(y)，则(!x.hashCode() == y.hashCode())。
    * 默认的实现是以x的内存地址作为对象。
    * 合法但不好的实现：总是返回17。
    * 基本数据类型的实现：Integer、Double、String、File、URL、Date……
    * 也可由用户自定义类型
4. Java库中的hash code
    * integers, booleans, doubles, strings
    ```java
    public final class Integer
    {
        private final int value;
        ...
        public int hashCode()
        { return value; }
    }
    public final class Boolean
    {
        private final boolean value;
        ...
        public int hashCode()
        {
            if (value) return 1231;
            else return 1237;
        }
    }
    public final class Double
    {
        private final double value;
        ...
        public int hashCode()
        {
            long bits = doubleToLongBits(value);
            return (int) (bits ^ (bits >>> 32));
        }
    }
    ```
    * strings
    ```java
    public final class String
    {
        private final char[] s;
        ...
        public int hashCode()
        {
            int hash = 0;
            for (int i = 0; i < length - 1; i++)
                hash = s[i] + (31 * hash);
            return hash;
        }// 若字符串长度为L，h=s[0]*31^(L-1)+s[1]*31^(L-2)+...+s[L-2]*31+s[L-1]
    }
    ```
    * strings的优化（将已计算过的hash value缓存，因为它是immutable类型，因此已经计算过的值不会改变）
    ```java
    public final int String
    {
        private int hash = 0;
        private string[] s;
        ...
        public int hashCode()
        {
            int h = hash;
            if (h != 0) return h;
            for (int i = 0; i < length - 1; i++)
                h = s[i] + (31 * h);
            hash = h;
            return h;
        }
    }
    ```
    * 用户自定义类型
    ```java
    public class Transaction implements Comparable<Transaction>
    {
        private final String who;
        private final Date when;
        private final double amount;
        public Transaction(String who, Date when, double amount)
        { /* as before */ }
        ...
        public boolean equals(Object y)
        { /* as before */ }
        public int hashCode()
        {
            int hash = 17;// 选择一个较小的素数
            hash = 31*hash + who.hashCode();
            hash = 31*hash + when.hashCode();
            // for refernce types, use hashCode()
            hash = 31*hash + (Double(amount)).hashCode(); //for primitive types, use hashCode() of wrapper type
            return hash;
        }
    }
    ```
    * 如何设计用户自定义类型的hash code
        * 通过$31x+y$将每个重要的字段连接起来。
        * 若字段为primitive类型，使用hashCode()的包装类型。
        * 若字段为null，返回0。
        * 若字段为reference类型，使用hashCode()。
        * 若字段为数组，将其应用到每一个下标中。
5. 取模的hash方法
    * hash code输出的是$-2^{31}$到$2^{31}-1$的一个整数，hash function输出的是$0$到$M-1$的一个整数，作为数组的索引，其中$M$通常取素数或者2的幂。
    * bug的做法
    ```java
    private int hash(Key key)
    { return key.hashCode() % M; }
    // hashCode()输出有可能是负值，但hash function输出的作为索引，必须为正值。
    ```
    * bug出现频率较少的做法
    ```java
    private int hash(Key key)
    { return Math.abs(key.hashCode()) % M; }
    ```
    // 输出仍有可能是负值。
    * 正确的做法
    ```java
    private int hash(Key key)
    { return (key.hashCode() & 0x7fffffff) % M; }
    ```
6. hashing均匀性假设
    * 假设所有键通过hash将被均匀地变成从$0$到$M-1$的整数
    * birthday problem：在$\sim\sqrt\frac{\pi M}{2}$次操作后，应有两个相同的索引值出现
    * coupon collector：在$\sim M\ln M$次操作后，每个索引值应该至少有一个元素。
    * load balancing：在$M$次操作后，最多元素的索引应至多有$\Theta(\log M/\log\log M)$个元素。

### Seperate Chaining

### Linear Probing

### Context
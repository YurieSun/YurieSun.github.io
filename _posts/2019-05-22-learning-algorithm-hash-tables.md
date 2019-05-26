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
1. 哈希冲突
    * 指两个不同的键经过hashing后得到相同的索引。
    * birthday problem表明，除非具有巨大的内存，否则难以避免冲突问题。
    * coupon collector + load balancing表明，冲突时均匀分布的。
2. separate-chaining synbol table
    * 采用一个大小为$M$的数组来表示$N$个链表（其中$M<N$）。
        * hash：将键映射为从$0$至$M-1$的整数$i$。
        * insert：将其插入到第$i$个链表的开头。
        * search：仅需搜索第$i$个链表。
    * java实现
    ```java
    public class SeparateChainingHash<Key, Value>
    {
        private int M = 97; //链表数量
        private Node[] st = new Node[M]; // 创建数组
        private static class Node
        {
            private Object key;
            private Object val;
            private Node next;
            // java中没有泛型的数组，因此声明为object类型
            ...
        }
        private int hash(Key key)
        { return (key.hashCode() & 0x7ffffff) % M; }
        public Value get(Key key)
        {
            int i = hash(key);
            for (Node x = st[i]; x != null; x = x.next)
                if (key.equals(x.key)) return (Value) x.val;
            return null;
        }
        public void put(Key key)
        {
            int i = hash(key);
            for (Node x = st[i]; x != null; x = x.next)
                if (key.equals(x.key)) { x.val = val; return; }
            st[i] = new Node(key, val, st[i]);
        }
    }
    ```
    * 数学分析
    * resizing
        * 目标：使得平均长度$\frac{N}{M}$是一个常数
        * 方法：当$\frac{N}{M}\ge 8$将数组大小$M$增大一倍；当$\frac{N}{M}\le 2$是将数组大小$M$缩小一半；当resizing时需要将所有键重新hash。
3. ST实现的总结
    <table>
    <tr>
        <td rowspan="2">implementation</td>
        <td colspan="3">guarantee</td>
        <td colspan="3">average case</td>
        <td rowspan="2">ordered ops?</td>
        <td rowspan="2">key interface</td>
    </tr>
    <tr>
        <td>search</td>
        <td>insert</td>
        <td>delete</td>
        <td>search hit</td>
        <td>insert</td>
        <td>delete</td>
    </tr>
    <tr>
        <td>sequential search<br>(unordered list)</td>
        <td>N</td>
        <td>N</td>
        <td>N</td>
        <td>N/2</td>
        <td>N</td>
        <td>N/2</td>
        <td>no</td>
        <td>equals()</td>
    </tr>
    <tr>
        <td>binary search<br>(ordered array)</td>
        <td>lg N</td>
        <td>N</td>
        <td>N</td>
        <td>lg N</td>
        <td>N/2</td>
        <td>N/2</td>
        <td>yes</td>
        <td>compareTo()</td>
    </tr>
    <tr>
        <td>BST</td>
        <td>N</td>
        <td>N</td>
        <td>N</td>
        <td>1.39 lg N</td>
        <td>1.39 lg N</td>
        <td>sqrt(N)</td>
        <td>yes</td>
        <td>compareTo()</td>
    </tr>
    <tr>
        <td>red-black BST</td>
        <td>2 lg N</td>
        <td>2 lg N</td>
        <td>2 lg N</td>
        <td>1.0 lg N</td>
        <td>1.0 lg N</td>
        <td>1.0 lg N</td>
        <td>yes</td>
        <td>compareTo()</td>
    </tr>
    <tr>
        <td>separate chaining</td>
        <td>lg N</td>
        <td>lg N</td>
        <td>lg N</td>
        <td>3-5</td>
        <td>3-5</td>
        <td>3-5</td>
        <td>no</td>
        <td>equals()<br>hashCode()</td>
    </tr>
    </table>

    * 其中separate chaining的性能是建立在均匀性假设的条件上。当键比较少且不需要ordered ops时，使用这种方法效率较高。

### Linear Probing
1. 基本思想
    * hash：将键映射成从$0$至$M-1$的整数$i$。
    * insert：若数组中的第$i$个位置为空，则放入；若不为空，则依次尝试$i+1$、$i+2$…
    * search：对于给定键，先映射成$i$，在数组中寻找第$i$个位置，若相等，则找到；若不相等，则依次尝试$i+1$、$i+2$…直到碰到空位，说明数组中没有该元素。
    * 注意：数组大小$M$必须要比键值对数量$N$大，且最好使用resizing以充分利用空间。
2. java实现
    ```java
    public class LinearProbingHashST<Key, Value>
    {
        private int M = 30001;
        private Value[] vals = (Value[]) new Object[M];
        private Key[] keys = (Keys[]) new Object[M];
        private int hash(Key key)
        { /* as before */ }
        private Value get(Key key)
        {
            for (i = hash(key); key[i] != null; i = (i+1) % M )
            // i的递增方式使得当元素到达数组末尾仍无空位时，可移动到开头的第一个位置，并继续往后移动寻找空位。
                if (key.equals(keys[i]))
                    return vals[i];
            return null;
        }
        public void put(Key key, Value val)
        {
            int i;
            for (i = hash(key); keys[i] != null; i = (i+1) % M)
                if (keys[i].equals(key))
                    break;
            keys[i] = key;
            vals[i] = val;
        }
    }
    ```
3. Knuth's parking problem
    * 模型  
    车到达一个含$M$个车位的单行道，每辆车想要的车位为一个随机数$i$，若$i$非空，则依次尝试$i+1$、$i+2$...
    * 分析  
    每辆车的平均寻找次数是多少？  
    half-full：若开始有$\frac{M}{2}$辆车，则平均寻找次数为$\sim\frac{3}{2}$。
    full：若开始有$M$辆车，则平均寻找次数为$\sim\sqrt{\frac{\pi M}{8}}$。
4. 数学分析
    * 在均匀性假设下，在一个数量为$M$、含$N=\alpha M$个键的linear probing哈希表中，平均寻找次数为：  
    search hit: $\sim \frac{1}{2}(1+\frac{1}{1-\alpha})$  
    search miss/insert: $\sim\frac{1}{2}(1+\frac{1}{{1-\alpha}^2})$
    * 参数$M$的选择：若$M$太大，则有很多空位，浪费储存空间；若$M$太小，则搜索时间急剧增加。因此，常选择$\alpha=\frac{N}{M}\sim\frac{1}{2}$，这样的话，search hit的平均次数约为$\frac{3}{2}$，search miss的平均次数约为$\frac{5}{2}$。
5. resizing
    为使得平均长度$\frac{N}{M}\le\frac{1}{2}$，当$\frac{N}{M}\ge\frac{1}{2}$时，数组大小$M$增加一倍；当$\frac{N}{M}\le\frac{1}{8}$时，数组大小$M$减小一半。
6. ST实现的总结
    <table>
    <tr>
        <td rowspan="2">implementation</td>
        <td colspan="3">guarantee</td>
        <td colspan="3">average case</td>
        <td rowspan="2">ordered ops?</td>
        <td rowspan="2">key interface</td>
    </tr>
    <tr>
        <td>search</td>
        <td>insert</td>
        <td>delete</td>
        <td>search hit</td>
        <td>insert</td>
        <td>delete</td>
    </tr>
    <tr>
        <td>sequential search<br>(unordered list)</td>
        <td>N</td>
        <td>N</td>
        <td>N</td>
        <td>N/2</td>
        <td>N</td>
        <td>N/2</td>
        <td>no</td>
        <td>equals()</td>
    </tr>
    <tr>
        <td>binary search<br>(ordered array)</td>
        <td>lg N</td>
        <td>N</td>
        <td>N</td>
        <td>lg N</td>
        <td>N/2</td>
        <td>N/2</td>
        <td>yes</td>
        <td>compareTo()</td>
    </tr>
    <tr>
        <td>BST</td>
        <td>N</td>
        <td>N</td>
        <td>N</td>
        <td>1.39 lg N</td>
        <td>1.39 lg N</td>
        <td>sqrt(N)</td>
        <td>yes</td>
        <td>compareTo()</td>
    </tr>
    <tr>
        <td>red-black BST</td>
        <td>2 lg N</td>
        <td>2 lg N</td>
        <td>2 lg N</td>
        <td>1.0 lg N</td>
        <td>1.0 lg N</td>
        <td>1.0 lg N</td>
        <td>yes</td>
        <td>compareTo()</td>
    </tr>
    <tr>
        <td>separate chaining</td>
        <td>lg N</td>
        <td>lg N</td>
        <td>lg N</td>
        <td>3-5</td>
        <td>3-5</td>
        <td>3-5</td>
        <td>no</td>
        <td>equals()<br>hashCode()</td>
    </tr>
    <tr>
        <td>linear probing</td>
        <td>lg N</td>
        <td>lg N</td>
        <td>lg N</td>
        <td>3-5</td>
        <td>3-5</td>
        <td>3-5</td>
        <td>no</td>
        <td>equals()<br>hashCode()</td>
    </tr>
    </table>

    * 其中separate chaining和linear probing的性能均是建立在均匀性假设的条件上。

### Context
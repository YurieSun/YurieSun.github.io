---
layout: post
title: 算法笔记（八）
description: "学习资料：algorithm (Princeton University)"
keyword: test
catogery: learning
tag: [algorithm]
---

## Symbol Tables

### API
1. 数据结构  
    是一个键-值对的抽象表达，基本操作有：插入一个含键的值，找出给定键的值。
2. 基本API
    ```java
    public class ST<Key, Value>
        ST() //创建符号表
        void put(Key key, Value val) //插入键值对
        Value get(Key key) //获得给定键的值
        void delete(Key key) //删除键值对
        boolean contains(Key key) //是否有含该键
        boolean isEmpty() //是否为空
        int size() //返回键值对数量
        Iterable<Key> keys //符号表中的所有键值
    ```
3. 约定
    * 值是非空的（即不会插入不含值的键）;  
    若键不存在get()方法返回null;  
    put()方法在插入已有值的键值对时会将原来的覆盖掉。
    * 原因  
        容易实现contains()。   
        ```java
        public boolean contains(Key key)
        { return get(key) != null; }
        ```  
       容易实现delete()。
        ```java
        public coid delete(Key key)
        { put(key, null); }
        ```
4. 键与值的数据类型
    * 值  
    任意泛型
    * 键：多种假设  
    若键是Comparable，使用compareTo()；  
    若键是任意泛型，使用equals()测试是否相等；  
    若键是任意泛型，使用equals()测试是否相等，使用hashCode()增加随机性。equals()和hashCode()均内置于java。
    * 符号表中的键最好采用immutable类型。
5. 相等测试
    * 所有java中的类都继承equals()方法。
    * java中相等满足以下性质：  
    自反性：x.equals(x) is true  
    对称性：x.equals(y) iff y.equals(x)  
    传递性：if x.equals(y) and y.equals(z), then x.equals(z)  
    非空性：x.equals(null) is faulse  
    * 实现  
    默认：检查(x==y)是否成立  
    客户端提供：Integer, Double, String, java.io.File, ...  
    用户自定义：需小心（参考下面的示例）
6. 用户自定义类型的相等判断
    ```java
    public final class Date implements Comparable<Date>
    // 使用equals()来继承是不安全的，这将破坏对称性
    {
        private final int month;
        private final int day;
        private final int year;
        ...

        public boolean equals(Object y)// 必须使用Object
        {
            if (y == this) return true;
            // 检验是否为相同的元素
            if (y == null) return false;
            // 检验空元素
            if (y.getClass() != this.getClass())
                return false;
            //两个object必须是同一个类
            Date that = (Date) y;// 该映射保证操作成功
            if (this.day != that.day) return false;
            if (this.month != that.month) return false;
            if (this.year != that.year) return false;
            return true;
            // 检验所有重要的字段是否相等
        }
    }
    ```
7. 相等的设计
    * 用户自定义的设计标准  
    对引用相等的优化；  
    检验空元素；  
    检验两元素是否为同一类型并进行映射；  
    比较所有重要的字段是否相等：若字段为基本数据类型，使用==；若字段为对象，使用equals()；若字段为数组，对每个下标都要检验。
    * 建议  
    不需使用依赖于其他字段的计算字段；  
    比较最能区分开的字段；  
    将compareTo()和equals()保持一致，即x.equals(y) iff (x.compareTo(y) == 0)；那么，若数据为Comparable，可使用compareTo()，若不是，可使用equals()。
8. 符号表的测试代码
    ```java
    public static void main(String[] args)
    {
        ST<String, Integer> st = new ST<String, Integer>();
        for (int i = 0; !StdIn.isEmpty; i++)
        {
            String key = StdIn.readString();
            st.put(key, i);
        }
        for (String s : st.keys())
            StdOut.println(s + " " + st.get(s));
    }
    ```
9. 计数器的实现
    ```java
    //读入一系列字符并将出现频率最高的单词打印出来
    public class FrequencyCounter
    {
        public static void main(String[] args)
        {
            int minlen = Integer.parseInt(args[0]);
            ST<String, Integer> st = new ST<String, Integer>();
            while (!StdIn.isEmpty())
            {
                String word = StdIn.readString();
                if (word.length() < minlen) continue;// 忽略短字符
                if (!st.contains(word)) st.put(word, 1);//第一次出现的字符
                else st.put(word, st.get(word) + 1);// 再次出现则增加次数
            } 
            String max = "";
            st.put(max, 0);
            for (String word : st.keys())
                if (st.get(word) > st.get(max))
                    max = word;
            StdOut.println(max + " " + st.get(max));
        }
    }
    ```

### Elementary Implementations
1. 链表的顺序搜索
    * 数据结构  
    维护未排序的键值对链表
    * 基本操作  
    search：扫描所有键直到找到  
    insert：扫描所有键直到找到，若未找到则插入
    * 需找到对search和insert均快速的实现方法。
2. 在有序数组中进行二分查找
    * 数据结构  
    维护一个有序的键值对数组
    * rank辅助函数  
    找出有多少个键$<k$
    * java实现
    ```java
    public Value get(Key key)
    {
        if (isEmpty()) return null;
        int i = rank(key);
        if (i < N && keys[i].compareTo(key) == 0) return vals[i];
        else return null;
    }
    private int rank(Key key)
    {
        int lo = 0, hi = N - 1;
        while (lo <= hi)
        {
            int mid = lo + (hi - lo) / 2;
            int cmp = key.compareTo(keys[mid]);
            if (cmp < 0) hi = mid - 1;
            else if (cmp > 0) lo = mid + 1;
            else return mid;
        }
        return lo;
    }
    ```
    * 存在的问题  
    每插入一个元素都需要将后面的元素后移，开销大。
3. 符号表基本实现的总结
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
        <td>log N</td>
        <td>N</td>
        <td>log N</td>
        <td>N/2</td>
        <td>yes</td>
        <td>compareTo()</td>
    </tr>
    </table>

### Ordered Operations
1. 有序符号表的API
    ```java
    public class ST<Key extends Comparable<Key>, Value>
        ST() //创建有序符号表
        void put(Key key, Value val) //插入键值对，当val为null时删除该键值对
        Value get(Key key) //返回该键对应的值
        void delete(Key key) //删除键值对
        boolean contains(Key key) //是否有与该键配对的值
        boolean isEmpty() //符号表是否为空
        int size() //键值对数量
        Key min() //最小的键
        Key max() //最大的键
        Key floor(Key key) //不大于该键的最大键
        Key ceiling(Key key) //不小于该键的最小键
        int rank(Key key) //比该键更小的键的数量
        Key select(int k) //第k个键
        void deleteMin() // 删除最小键
        void deleteMax() //删除最大键
        int size(Key lo, Key hi) //在[lo..hi]中键的个数
        Iterable<Key> keys(Key lo, Key hi) //在[lo..hi]中排序后的键
        Iterable<Key> keys() //表中所有的键，且已排序
    ```
2. 有序符号表的操作总结

    ||sequential search|binary search|
    |:-:|:-:|:-:|
    |search|$N$|$\lg N$|
    |insert/delete|$N$|$N$|
    |min/max|$N$|$1$|
    |floor/ceiling|$N$|$\lg N$|
    |rank|$N$|$\lg N$|
    |select|$N$|$1$|
    |ordered iteration|$N\lg N$|$N$|

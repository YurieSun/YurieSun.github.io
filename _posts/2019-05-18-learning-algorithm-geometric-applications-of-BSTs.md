---
layout: post
title: 算法笔记（十一）
description: "学习资料：algorithm (Princeton University)"
keywords: test
category: Learning
tags: [algorithm]
---

## Geometric Applications of BSTs

### 1d Range Search
1. 基本思想
    * 这是一个有序符号表的拓展，包括如下操作：
        * insert：插入键值对
        * search：找到键为$k$对应的值
        * delete：删除键$k$
        * range search：找到位于$k_1$和$k_2$之间的所有键
        * range count：找到位于$k_1$和$k_2$之间的键的数量
    * 应用：数据库查询
    * 几何中的应用：键是位于一条线上的点，range search/count可以找到在某个区间里面的点及其个数。
2. BST实现
    * 1d range count
    ```java
    public int size(Key lo, Key hi)
    {
        if (contains(hi)) return rank(hi) - rank(lo) + 1;
        else return rank(hi) - rank(lo);
        //rank(k)返回小于k的键的个数
    }
    ```
    * 1d range search  
    递归查找左子树所有的键，检验中间键，递归查找右子树左右的键。
3. 各实现方法的总结
    * 基本实现
        * 无序链表：insert快速，但range search很慢
        * 有序数组：insert很慢，但可以使用二分查找实现range search
    * 总结

    |data structure|insert|range count|range search|
    |:-:|:-:|:-:|:-:|
    |unordered list|$1$|$N$|$N$|
    |ordered array|$N$|$\log N$|$R+\log N$|
    |BST|$\log N$|$\log N$|$R+\log N$|
    其中，$N$表示键的总个数，$R$表示在范围内的键的个数。

### Line Segment Intersection

### Kd trees

### Interval Search Trees

### Rectangle Intersection
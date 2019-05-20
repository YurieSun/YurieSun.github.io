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
    * 总结（$N$表示键的总个数，$R$表示在范围内的键的个数。）

    |data structure|insert|range count|range search|
    |:-:|:-:|:-:|:-:|
    |unordered list|$1$|$N$|$N$|
    |ordered array|$N$|$\log N$|$R+\log N$|
    |BST|$\log N$|$\log N$|$R+\log N$|

### Line Segment Intersection
1. 问题描述  
    给定$N$条水平或垂直的线段，找到所有交点。  
    若采用最简单的方法，需要平方时间。
2. sweep-line算法
    * 基本思想：用一条垂直线段从左向右扫描，若碰到水平线段左端点，将其y坐标插入BST；若碰到水平线段右端点，将其y坐标从BST删去；若碰到垂直线段，则寻找在该线段的y坐标范围内有多少个点，即range search。因此将2d问题退化成1d的问题，可直接使用上一节的算法。
    * 若在$N$个正交线段中找到所有$R$个交点，该算法所需时间与$R+N\log N$成正比。

### Kd trees
1. 2d orthogonal range search
    * 将有序符号表拓展到2d。基本操作有：
        * insert a 2d key
        * search a 2d key
        * range search
        * range count
    * 应用：网络、电路设计、数据库……
    * 几何中的应用：其中的键表示平面上的点，因此range search/count是指找出在给定的二维范围即一个长方形中的点及个数。
    * 实现方法：grid implemetation
        * 将空间划分为$M\times M$的正方形网格
        * 创建每个网格包含点个数的列表
        * 使用二维数组来索引网格
        * insert：把(x,y)添加到列表中对应的网格
        * range search：检查与给定范围有交叉的网格
    * 数学分析
        * 时间与空间的权衡：花费空间为$M^2+N$，检查一个网格的平均时间为$1+\frac{N}{M^2}$。因此$M$不能取太大也不能取太小。根据经验，一般采用$\sqrt{N}\times\sqrt{N}$的网格。
        * 运行时间：若点在空间内是均匀分布的，且$M\sim\sqrt{N}$，则初始化数据结构的时间为$N$，插入点需要的时间为$1$，range search时在范围内的每个点所需时间为$1$。
2. 集群
    * grid implementation对于空间分布均匀的点来说是一种简单、快速的方法，但出现集群（即点集中在某个区域）现象时，这种方法就不太适用了。
    * space-partitioning trees：使用树来表示递归分割的2d空间。
        * 种类：2d tree（分割成两个半平面）、quadtree（分割成四个$\frac{1}{4}$平面）、BSP tree（分割成两个区域）
        * 应用：2d range search、nearest neighbor search等
    * 2d tree
        * 数据结构：将x、y坐标作为键的BST，每个insert操作相当于将平面分割一次，并且水平分割与垂直分割交替进行。
        * range search：检验节点上的点是否在给定范围内，递归搜索left/bottom，递归搜索right/top。
        * range search的数学分析：通常情况下的花销为$R+\log N$，最坏情况（即使树完全平衡）的花销为$R+\sqrt{N}$。
        * nearest neighbor：找到与给定点距离最近的点。计算节点上的点的距离，递归搜索left/bottom，递归搜索right/top，若出现更小的距离，则进行替代。
        * nearest neighbor的数学分析：通常情况下的花销为$\log N$，最坏情况下（即使树完全平衡）的花销为$N$。
3. kd tree
    * 将$k$维空间递归分割成两个半空间
    * 在处理高维数据时，这是一种高效、简单的数据结构。

### Interval Search Trees
1. 1d interval search的数据结构  
    储存有重叠的区间，基本操作有：  
    * insert：插入区间(lo, hi)
    * search：搜索区间(lo, hi)
    * delete：删除区间(lo, hi)
    * interval intersection query：给定区间(lo, hi)，找到所有（或其中一个）与之相交的区间。
2. API  
    假定没有任何两个区间有相同的左端点。
    ```java
    public class Interval1ST<Key extends Comparable<Key>, Value>
        interval1ST() //创建interval search tree
        void put(Key lo, Key hi, Value val) //向ST中插入interval-value pair
        Value get(Key lo, Key hi) //返回给定区间的键值
        void delete(Key lo, Key hi) //删除给定区间
        Iterable<Value> intersects(Key lo, Key hi) //返回所有与给定区间相交的区间
    ```
3. interval search trees的创建
    * BST中每个节点储存的一个区间(lo, hi)
    * 使用左端点作为键，在每个节点储存以该节点为根节点的所有子树中的最大端点值。
4. insertion

### Rectangle Intersection
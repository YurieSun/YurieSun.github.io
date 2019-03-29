---
layout: post
title: "算法笔记（一）"
description: "学习资料：algorithm (Princeton University)"
keywords: test
category: Learning
tags: [machine learning]
---


## 并查集（Union-Find）算法

### Dynamic Connectivity
1. 问题描述  
给定一系列个数为N的对象集，union操作为连接其中两个对象，find/connected query操作为查询任意两个对象是否是连通的。
2. 建模  
假设连通具有以下性质：   
（1）自反性：任一对象p与自身是连通的；  
（2）对称性：若p与q连通，则p与p也是连通的；  
（3）传递性：若p与q连通，q与r连通，则p与r连通。  
建立连通集：互相连通的最大的对象集。
3. 实现操作  
find：检查两对象是否在同一连通集中；  
union：将这两个对象所在的连通集替换为和并后的连通集。
4. 数据结构  
```bash
UF(int N) //含N个对象的的初始化集合  
void union(int p, int q) //union操作
boolean connected(int p, int q) //connected操作
```
6. 实现可用算法的基本步骤  
（1）对问题建模；  
（2）找出一种算法来解决问题；  
（3）这个算法是否足够快且占用内存合理；  
（4）若不是，分析原因；  
（5）解决问题；  
（6）多次迭代直到满足要求。

### Quick Find
实现Union-Find的第一种方法：Quick-find。
1. 数据结构  
长度为N的整数数组id[]；  p和q的id相等时为连接。
2. 基本操作  
```java
void union(int p, int q) //将p的id设为q的id;  
boolean connected(int p, int q) //检验p和q的id是否相等。
```
3. Java实现
```java
public class QuickFindUF
{
    private int[] id;

    public QuickFindUF(int N)
    {
        id = new int[N];
        for (int i = 0;i < N; i++)
            id[i]=i;
    }

    public boolean connected(int p, int q)
    {
        return id[p] == id[q];
    }

    public void union(int p, int q)
    {
        int pid = id[p];
        int qid = id[q];
        for (int i = 0; i < id.length; i++)
            if (id[i] == pid) id[i] = qid; //若不先取出p的id，会有bug存在？
    }
}
```
4. 速度  
find操作很快，但若对N个对象进行N次union操作，需$N^2$时间；对于平方算法，当数据非常多时，所花时间无法估量，因此，需避免平方算法。

### Quick Union
Quick-union是一种比Quick-find更好的方法。  
1. 数据结构  
长度为N的整数数组id[]；  
id[i]是i的父节点，则i的根节点为id[id[id[...id[i]...]]]。
2. 基本操作  
int root(int i) //返回i的根节点；
void union(int p, int q) //将p的根节点设为q的根节点;  
boolean connected(int p, int q) //检验p和q的根节点是否相等。
3. Java实现
```java
public class QuickUnionUF
{
    private int[] id;

    public QuickUnionUF(int N)
    {
        id = new int[N];
        for (int i == 0; i < N; i++)
            int[i] = i;
    }

    private int root(int i)
    {
        while (i != id[i]) i = id[i];
        return i;
    }

    public boolean connected(int p, int q)
    {
        return root(p) == root(q);
    }

    public void union(int p, int q)
    {
        int i = root(p);
        int j = root(q);
        id[i] = j;
    }
}
```
4. 速度  
connected操作在最坏情况下可能需要N次数组访问，仍然较慢。
5. Quick-find和Quick-union的缺点  
Quick-find：union需要N次数组访问；tree完全扁平，若变高代价大；
Quick-union：connected在最坏情况下需要N次数组访问；tree有可能变高。

### Improvements
1. 改进方法一：weighing（加权：避免将大树放在小树下面）  
 * java实现  
 其中初始化、root、connected均相同；union操作需多维护一个数组储存根节点下的子节点个数，以确保小树放在大树下，则只需加入以下代码：
```java
if (i == j) return;
if (sz[i] < sz[j]) {id[i] = j; sz[j] += sz[j]; }
else               {id[j] = i; sz[i] += sz[j]; }
```
* 速度  
connected操作：所花时间与p和q的高度成正比，而任一节点的高度不会超过 lg N；
union操作： 若已知根节点，所花时间为常数，而包括寻找根节点的时间为 lg N。

2. 在weighted Quick-Union基础上的改进：path compression  
* 法一：在找p的根节点时，将所有被检查的节点连接到其根节点上，则在root()中增加一个循环；  
法二：在找根节点时，将路径上的所有节点连接到其祖父节点上，则只需在root()增加一行代码，如下：
```java
private root(int i)
{
    while (i != id[i])
    {
        id[i] = id[id[i]];
        i = id[i];
    }
    return i;
}
```
* 速度  
选用第二种方法，理论上weighted quick-union with path compression (WQUPC)并不是线性的，在实际运用中，WQUPC是线性的。  
经证明，该问题并不存在线性算法。

### Applications  
* Percolatopn （渗透问题，如Monte Carlo模拟）
* Least common ancestor （最近公共祖先，LCA）
* Hoshen-Kopelman algorithm in physics
* Kruskal's minimum spanning tree algorithm
* Compeling eauivalence statements in Fortran
* Matlab's bwlabel() function in image processing
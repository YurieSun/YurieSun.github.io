---
layout: post
title: 算法笔记（十六）
description: "学习资料：algorithm (Princeton University)"
keywords: test
category: Learning
tags: [algorithm]
---

## Minimum Spanning Trees
### Introduction
1. 生成树   
    G的一个生成树是指具有以下性质的一个子图T：均相连、无回路、包括所有点。
2. 最小生成树  
    给定一个无方向的图，其中每条边均有正值的权重，目标是找到一个权重最小的生成树。

### Greedy algorithm
1. 预备知识
    * 简化假设  
    图是相连的，且每条边的权重不同。
    * cut
        * cut是指将图中的点划分为两个非空子集。
        * crossing edge是指连接两个子集中的点的边。
        * cut的性质：对于任意cut，具有最小权重的crossing edge是在MST中的。
2. greedy algorithm
    * 方法
        * 所有边均视为灰色；
        * 寻找没有黑色crossing edges的cut，并将其中最小权重的crossing edge标为黑色；
        * 重复直至有$V-1$条边被标为黑色。
    * 证明
        * 任意标为黑色的边一定在MST中（通过cut的性质可知）；
        * 若黑边小于$V-1$，则存在没有黑色crossing edges的cut。
    * 实现  
        * 如何进行cut？  
        Kruskal's algorithm、Prim's algorithm
        * 如何找到最小权重的边？
    * 若去掉两个假设，greedy algorithm仍然是正确的。
        * 当相同权重存在时，greedy MST algorithm仍然正确；
        * 当图不相连时，则计算最小生成森林，即计算每个component的MST。

### Edge-Weighted Graph API
1. weighted edge
    * API
    ```java
    public class Edge implements Comparable<Edge>
        Edge(int v, int w, double weight)
        int either //返回边的任意一个端点
        int other(v) //返回边中除v之外的端点
        int compareTo(Edge that) //比较边的权重
        //optional
        double weight()
        String toString()
    ```
    * java实现
    ```java
    public class Edge implements Comparable<Edge>
    {
        private final int v, w;
        private final double weight;
        public Edge(int v, int w, double weight)
        {
            this.v = v;
            this.w = w;
            this.weight = weight;
        }
        public int either()
        { return v; }
        public int other(int v)
        {
            if (vertex == v) return w;
            else return v;
        }
        public int compareTo(Edge that)
        {
            if (this.weight < that.weight) return -1;
            else if (this.weight > that.weight) return +1;
            else return 0;
        }
    }
    ```
2. edge-weighted graph
    * API
    ```java
    public class EdgeWeightedGraph
        EdgeWeightedGraph(int V)
        void addEdge(Edge e)
        Iterable<Edge> adj(int v)
        //optional
        EdgeWeightedGraph(In in)
        Iterable<Edge> edges() //所有的边
        int V()
        int E()
        String toString()
    ```
    * java实现  
    仍用adjaceny-lists表示edge-weighted graph。
    ```java
    public class EdgeWeightedGraph
    {
        private final int V;
        private final Bag<Edge>[] adj;
        public EdgeWeightedGraph(int V)
        {
            this.V = V;
            adj = (Bag<Edge>[]) new Bag[V];
            for (int v = 0; v < V; v++)
                adj[v] = new Bag<Edge>();
        }
        public void addEdge(Edge e)
        {
            int v = e.either(), w = e.other(v);
            adj[v].add(e);
            adj[w].add(e);
        }
        public Iterable<Edge> adj(int v)
        { return adj[v]; }
    }
    ```
3. minimum spanning tree
    * API
    ```java
    public class MST
        MST(EdgeWeightedGraph)
        Iterable<Edge> edges() //MST中的所有边
        //optional
        double weight() //MST的总权重
    ```
    * test code
    ```java
    public static void main(String[] args)
    {
        In in = new In(arg[0]);
        EdgeWeightedGraph G = new EdgeWeightedGraph(in);
        MST mst = new MST(G);
        for (Edge e : mst.edges())
            StdOut.println(e);
        StdOut.printf("%.2f\n", mst.weight());
    }
    ```

### Kruskal's Algorithm

### Prim's Algorithm

### Context
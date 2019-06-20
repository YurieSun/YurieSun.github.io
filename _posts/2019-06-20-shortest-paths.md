---
layout: post
title: 算法笔记（十七）
description: "学习资料：algorithm (Princiton University)"
keywords: test
category: Learning
tags: [algorithm]
---

## Shortest Paths
### APIs
1. 最短路径问题描述  
    给定一个edge-weighted digraph，找到从s到t的最短路径。
2. weighted directed edge 
    * API
    ```java
    public class DirectedEdge
        DirectedEdge(int v, int w, double weight)
        int from()
        int to()
        double weight()
        //optional
        String toString()
    ```
    * java实现
    ```java
    public class DirectedEdge
    {
        private final int v, w;
        private final double weight;
        public DirectedEdge(int v, int w, double weight)
        {
            this.v = v;
            this.w = w;
            this. weight = weight;
        }
        public int from()
        { return v; }
        public int to()
        { return w; }
        public double weight()
        { return weight; }
    }
    ```
2. edge-weighted digraph 
    * API
    ```java
    public class EdgeWeitedDigraph
        EdgeWeightedDigraph(int V)
        void addEdge(DirectedEdge e)
        Iterable<DirectedEdge> adj(int v)
        int V()
        //optional
        EdgeWeightedDigraph(In in)
        int E()
        Iterable<DirectedEdge> edges()
        String toString()
    ```
    * 使用adjacency-lists表示的java实现（与EdgeWeightedGraph完全一样，除了更改函数名、变量名）
    ```java
    public class EdgeWeightedDigraph
    {
        private final int V;
        private final Bag<DirectedEdge>[] adj;
        public EdgeWeightedDigraph()
        {
            this.V = V;
            adj = (Bag<DirectedEdge>[]) new Bag[V];
            for (int v = 0; v < V; v++)
                adj[v] = new Bag<DirectedEdge>();
        }
        public void addEdge(DirectedEdge e)
        {
            int v = e.from();
            adj[v].add(e);
        }
        public Iterable<DirectedEdge> adj(int v)
        { return adj[v]; }
    }
    ```
3. single-source shortest paths
    * API
    ```java
    public class SP
        SP(EdgeWeightedDigraph G, int s)
        double distTo(int v)
        Iterable<DirectedEdge> pathTo(int v)
        //optional
        boolean hasPathTo(int v)
    ```
    * test client
    ```java
    SP sp = new SP(G, s);
    for (int v = 0; v < V; v++)
    {
        StdOut.printf("%d to %d (%.2f): ", s, v, sp.distTo(v));
        for (DirectedEdge e : sp.pathTo(v))
            StdOut.print(e + " ");
        StdOut.println();
    }
    ```

### Shortest-Paths Properties
1. single-source shortest paths的数据结构
    * 目标  
    找到从点s到其余所有点的最短路径
    * 存在最短路径树(shortest-paths trees, SPT)  
    通过两个以点为索引的数组来表示SPT：distTo[v]表示从s到v的最短路径的长度，edgeTo[v]表示从s到v的最短路径中v的直接到达v的上一条路径。
    ```java
    public double distTo(int v)
    { return distTo[v]; }
    public Iterable<DirectedEdge> pathTo(int v)
    {
        Stack<DirectedEdge> path = new  Stack<DirectedEdge>;
        for (DirectedEdge e = edgeTo[v]; e != null; e = edgeTo[e.from])
            path.push(e);
        return path;
    }
    ```
2. edge relaxation
    * relax edge $e=v\to w$
        * distTo[v]表示从s到v的最短路径的距离；
        * distTo[w]表示从s到w的最短路径的距离；
        * edgeTo[w]表示从从s到w的最短路径中v的直接到达w的上一条路径。
        * 若$e=v\to w$通过v给出更短的路径，则同时更新distTo[w]和edgeTo[w]。
    ```java
    private void relax(DirectedEdge e)
    {
        int v = e.from(), w = e.to();
        if (distTo[w] > distTo[v] + e.weight())
        {
            distTo[w] = distTo[v] + e.weight;
            edgeTo[w] = e;
        }
    }
    ```
3. shortest-paths optimality conditions
    * 记G为一个edge-weighted digraph，则distTo[]成为从s开始的最短路径的长度，当且仅当：
        * distTo[s] = 0;
        * 对于每个点v，distTo[v]都是从s到v的某条路径长度；
        * 对于每条边$e=v\to w$，distTo[w] $\le$ distTo[v] + e.weight()。
    * 证明  
        若存在某条边满足distTo[w] > distTo[v] + e.weight()，则e将通过v给出从s到w的路径，其长度小于distTo[w]。
4. generic shortest-paths algorithm
    * 解决方案
    ```java
    Genetic algorithm (to compute SPT from s)
        Initialize distTo[s] = 0 and distTo[v] = infinity for other vertices.
        Repeat until optimality conditions are satisfied:
          -Relax any edge.
    ```
    * 实现方法：如何去选择对哪条边进行relax操作
        * Dijkstra's algorithm (nonnegtive weights)
        * Topological sort algorithm (no directed cycles)
        * Bellman-Ford algorithm (no negative cycles)

### Dijkstra's Algorithm

### Edge-Weighted DAGs

### Negative Weights
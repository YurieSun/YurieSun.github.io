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
1. 方法  
    将边按照权重大小升序排列，依次将边放入MST中，若形成回路则忽略该边，直到找到$V-1$条边。
2. 正确性证明  
    * Kruskal's algorithm可以计算出MST，因为该算法是greedy MSTalgorithm的一个特例。
    * 假设Kruskal's algorithm将边$e=v-w$设为黑色，则可将cut视为将图分为两部分：与$v$相连的点集、其余点形成的集合。不存在黑色的crossing edge，且不存在权重更小的crossing edge。
3. 挑战与解决方法  
    如何检测将边$v-w$加入后形成了回路？
    * 若从v开始运行DFS，检测是否能到达w，则需要时间为$V$。
    * 若使用union-find，则需要时间仅为$\log V$。
    * 因此，采用union-find数据结构。维护在树中的每一个connected component集合。若$v$和$w$在同一个集合，则$v-w$将形成回路；若不在，则将含有$v$和$w$的集合合并，从而将$v-w$加入MST中。
4. java实现
    ```java
    public class KruskalMST
    {
        private Queue<Edge> mst = new Queue<Edge>();
        public KruskalMST(EdgeWeightedGraph G)
        {
            MinPQ<Edge> pq = new MinPQ<Edge>(G.edges());
            UF uf = new UF(G.V());
            while (!pq.isEmpty() && mst.size() < G.V() - 1)
            {
                Edge e = pq.delMin();
                int v = e.either(), w = e.other(v);
                if (!uf.connected(v, w))
                {
                    uf.union(v, w);
                    mst.enqueue(e);
                }
            }
        }
        public Iterable<Edge> edges()
        { return mst; }
    }
    ```
5. 运行时间
    * Kruskal's algorithm计算MST的时间与$E\log E$成正比（在最坏情况下）。

    |operation|frequency|time per op|
    |:-:|:-:|:-:|
    |build pq|$1$|$E$|
    |delete-min|$E$|$\log E$|
    |union|$V$|$\log^* V$|
    |connected|$E$|$\log^* V$|
    * 其中，$\log^* V\le5$。若边已经是有序的，则时间变为$E\log^*V$。

### Prim's Algorithm
1. 方法  
    从点0开始，将与MST中其中一个点相连的边当中权重最小的边加入MST中，重复直到MST中有$V-1$条边。
2. 正确性证明
    * Prim's algorithm可以计算出MST，因为该算法是greedy MSTalgorithm的一个特例。
    * 假设边$e$是连接MST中的点和不在MST中的点的一条边且具有最小权重，则可将cut视为将图分为两部分：与$v$相连的点集、其余点形成的集合。不存在黑色的crossing edge，且不存在权重更小的crossing edge。
3. 挑战与解决方法  
    如何找到仅与MST中一个点相连的具有最小权重的边？
    * 若将所有边尝试一遍，所需时间为$E$；
    * 若使用priority queue，则所需时间变为$\log E$。
    * 因此，采用priority queue的数据结构。(lazy solution)维护一个至少含MST中一个点的边的PQ，其中，key指边，priority指边的权重。通过delete-min方法删除边$e=v-w$并加入到MST，若$v$和$w$均被标记，即均在MST中，则忽略；否则，假设$w$是不在MST中的点，则将与$w$相连的所有边都加入PQ，同时将边$e$加入到MST，并将$w$进行标记。
4. lazy implementation
    ```java
    public class LazyPrimMST
    {
        private boolean[] marked;
        private Queue<Edge> mst;
        private MinPQ<Edge> pq;
        public LazyPrimMST(WeightedGraph G)
        {
            pq = mew MinPQ<Edge>();
            mst = Queue<Edge>();
            marked = new boolean[G.V()];
            visit(G, 0);
            while (!pq.isEmpty() && mst.size < G.V() - 1)
            {
                Edge = pq.delMin();
                int v = e.either(), w = e.other(v);
                if(marked[v] && marked[w]) continue;
                mst.enqueue(e);
                if (!marked[v]) visit(G, v);
                if (!marked[w]) visit(G, w);
            }
        }
        private void visit(WeightedGraph G, int v)
        {
            marked[v] = true;
            for (Edge e : G.adj(v))
                if (!marked[e.other(v)])
                    pq.insert(e);
        }
        public Iterable<Edge> mst()
        { return mst; }
    }
    ```
    * 运行时间  
    lazy Prim's algorithm计算MST的时间与$E\log E$成正比，且额外需要的空间与$E$成正比（在最坏情况下）。

    |operation|frequency|binary heap|
    |:-:|:-:|:-:|
    |delete min|$E$|$\log E$|
    |insert|$E$|$\log E$|
5. eager implementation
    * 挑战  
    找到仅与MST中一个点相连且权重最小的边。
    * 解决方法  
    维护一个与MST通过边相连的点的PQ，其中对于PQ中的每个点最多只有一个索引。而点$v$的priority是指通过$v$与MST相连的最短边的权重。具体步骤是，用delete min方法将最小点$v$从PQ中删除，并将与其相关的边$e=v-w$加入到MST中；考虑所有与$v$相连的边$e=v-x$并更新PQ：若$x$已在MST中，则忽略；若$x$不在，则加入到PQ中；若$v-x$成为通过$x$与MST相连的最短边，则降低对应的priority。
    * indexed priority queue
        * 将PQ中每个键对应到$0$至$N-1$的索引，从而可提供insert和delete-the-minimum方法，且对给定的键的索引提供decrease-key方法。
        ```java
        public class IndexMinPQ<Key extends Comparable<Key>>
            IndexMinPQ(int N) //创建索引为0到N-1的indexed PQ
            void insert(int i, Key key) //将索引i与key相连
            void decreaseKey(int i, Key key) //减小与索引i相连的key
            boolean contains(int i)
            int delMin()
            boolean isEmpty()
            int size()
        ```
        * binary heap implementation  
        与MinPQ的代码相同，维护多个数组keys[]、pq[]和qp[]，其中keys[i]是i的priority，pq[i]是heap position为i的键对应的索引，qp[i]是具有索引i的键的heap position；使用swim(qp[i])来实现decreaseKey(i, key)。
    * 使用不同PQ实现方法  
    对于密集的图，使用数组实现较好；而对于稀疏的图，binary heap更快；在性能较重要的情况下，值得使用4-way heap；而Fibonacci heap在理论上很不错，但难以实现。

    |PQ implementation|insert|delete-min|decrease-key|total|
    |:-:|:-:|:-:|:-:|:-:|
    |unordered array|$1$|$V$|$1$|$V^2$|
    |binary heap|$\log V$|$\log V$|$\log V$|$E\log V$|
    |d-way heap|$\log_d V$|$\log_d V$|$\log_d V$|$E\log_{E/V} V$|
    |Fibonacci heap|$1$|$\log V$|$1$|$E+V\log V$|

### Context
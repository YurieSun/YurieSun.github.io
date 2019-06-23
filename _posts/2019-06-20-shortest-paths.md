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
1. 方法
    * 以到s的距离为升序，考虑每一个点；
    * 将点加入树中，并将以该点开始的边进行relax操作。
2. 正确性证明  
    为什么Dijkstra's algorithm可以计算任意edge-weighted digraph with nonnegatuve weights的SPT？
    * 每条边$e=v\to w$仅进行一次relax操作（当点v被relax时），使得distTo[w] $\le$ distTo[v] + e.weight()。
    * 以上不等式会一直成立直至算法终止，理由如下：
        * distTo[w]不会增加；（distTo[]是不断减小的）
        * distTo[v]不会再变化。（每一步都会选择最小的distTo[]，且边的权重为非负的）
3. java实现
    ```java
    public class DijkstraSP
    {
        private DirectedEdge[] edgeTo;
        private double[] distTo;
        private IndexMinPQ<Double> pq;
        public DijkstraSP(EdgeWeightedDigraph G, int s)
        {
            edgeTo = new DirectedEdge[G.V()];
            distTo = new double[G.V()];
            pq = new IndexMinPQ<Double>(G.V());
            for (int v = 0; v < V; v++)
                distTo[v] = Double.POSITIVE_INFINITY;
            distTo[s] = 0;
            pq.insert(s, 0.0);
            while (!pq.isEmpty())
            {
                int v = pq.delMin();
                for (DirectedEdge e : G.afj(v))
                    relax(e);
            }
        }
        private void relax(DirectedEdge e)
        {
            int v = e.from(), w = e.to();
            if (distTo[w] > distTo[v] + e.weight)
            {
                distTo[w] = distTo[v] + e.weight;
                edgeTo[w] = e;
                if (pq.contains(w)) pq.decreaseKey(w, distTo[w]);
                else pq.insert(w, distTo[w]);
            }
        }
    }
    ```
4. 不同PQ实现方法  
    * 对于密集的图，使用数组实现较好；而对于稀疏的图，binary heap更快；在性能较重要的情况下，值得使用4-way heap；而Fibonacci heap在理论上很不错，但难以实现。

    |PQ implementation|insert|delete-min|decrease-key|total|
    |:-:|:-:|:-:|:-:|:-:|
    |unordered array|$1$|$V$|$1$|$V^2$|
    |binary heap|$\log V$|$\log V$|$\log V$|$E\log V$|
    |d-way heap|$\log_d V$|$\log_d V$|$\log_d V$|$E\log_{E/V} V$|
    |Fibonacci heap|$1$|$\log V$|$1$|$E+V\log V$|
5. 与spanning tree对比
    * Dijkstra's algorithm与Prim's algorithm基本相同，它们都算是计算spanning tree的算法。
    * 不同的是，Prim算法是通过undirected edge找离树最近的点，Dijkstra算法是通过directed path找离图最近的点。
    * DFS和BFS也属于计算spanning tree的算法。

### Edge-Weighted DAGs
1. 方法  
    当图中没有回路时，可以用更简单的方法找到最短路径。以topological order考虑各点，并将所有以该点为起点的边进行relax操作。
2. 正确性证明  
    为什么topological order algorithm可以计算任意edge-weighted DAG（权重不能为负）的SPT，且所需时间与$E+V$成正比？
    * 每条边$e=v\to w$仅进行一次relax操作（当点v被relax时），使得distTo[w] $\le$ distTo[v] + e.weight()。
    * 以上不等式会一直成立直至算法终止，理由如下：
        * distTo[w]不会增加；（distTo[]是不断减小的）
        * distTo[v]不会再变化。（由于topological order，在v被relax之后不会再有指向v的边被relax）
3. java实现
    ```java
    public class AcyclicSP
    {
        private DirectedEdge[] edgeTo;
        private double[] distTo;
        public AcyclicSP(EdgeWeightedDigraph G, int s)
        {
            edgeTo = new DirectedEdge[G.V()];
            distTo = new double[G.V()];
            for (int v = 0; v < V; v++)
                distTo[v] = Double.POSITIVE_INFINITY;
            distTo[s] = 0.0;
            Topological topological = new Topological(G);
            for (int v : topological.order())
                for (DirectedEdge e : G.adj(v))
                    relax(e); 
        }
    }
    ```
4. 应用
    * content-aware resizing
        * 目标  
        在手机或浏览器上对图片进行自适应，且没有扭曲。
        * 方法  
        找到垂直或水平方向上的一条权重最小的路径并删除。  
        将照片看成grid DAG，其中点为像素，边为每个像素与其下方3个点的连线，权重则是每个点周围8个点的能量函数，找到从上之下的最短路径。
    * longest paths in edge-weighted DAGs
        * 可将其转换成最短路径问题，即将所有权重变成其相反数，找到最短路径，再将总权重变成其相反数，得到的便是权重和最大的路径，即最长路径。
        * 可用于parallel job scheduling中。

### Negative Weights
1. negative cycles
     * 定义  
     negative cycle指一个回路的总权重为负数。
     * 当且仅当图中不含negative cycle时才存在SPT。
2. Bellman-Ford algorithm
    ```java
    Initialize distTo[s] = 0 and distTo[v] = infinity for all other vertices.
    Repeat V times:
      -Relax each edge.
    ```
    * 对于任意edge-weighted digraph with no negative cycles，使用动态算法计算SPT，所需时间与$E\times V$成正比。
    * 提高速度的措施
        * 若在第i个循环中，distTo[v]不再变化，则在第i+1个循环中，不需要对以v为起点的边进行relax操作了。  
        * 使用FIFO实现，即维护一个distTo[]不再变化的点的队列。
        * 效果：在最坏情况下运行时间与$E\times V$成正比，但在实际应用中则要快很多。
3. single-source shortest-paths implementation: cost summary
    * 有回路或存在负权重使得问题变难；而负值回路则使得问题无法解决。

    |algorithm|restriction|typical case|worst case|extra space|
    |:-:|:-:|:-:|:-:|:-:|
    |topological sort|no directed cycles|$E+V$|$E+V$|$V$|
    |Dijkstra(binary heap)|no negative weights|$E\log V$|$E\log V$|$V$|
    |Bellman-Ford|no negatuve cycles|$EV$|$EV$|$V$|
    |Bellman-Ford|no negative cycles|$E+V$|$EV$|$V$|
4. 寻找negative cycle
    * 在SP的API中加入2个方法
    ```java
    boolean hasNegativeCycle()
    Iterable <DirectedEdge> negativeCycle()
    ```
    * 若存在negative cycle，则Bellman-Ford方法会一直在循环中出不来，并不断更新distTo[]和edgeTo[]。因此，若在第V个循环中，若点v在不断更新，则说明存在negative cycle，并可以通过edgeTo[v]找到。在实际应用中，需要更加频繁地检查是否存在negative cycles。
    * 应用：arbitrage detection
5. 总结
    * nonnegative weights  
    在许多应用中都会出现，Dijkstra' algorithm接近线性时间。
    * acyclic edge-weighted digraphs  
    在某些应用中会出现，topological algorithm是线性时间，且权重值可为负。
    * negative weights and negative cycles  
    在某些应用中会出现，若不存在negative cycle，则可以通过Bellman-Ford找到最短路径；若存在negative cycle，则可通过Bellman-Ford找到一个回路。
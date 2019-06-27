---
layout: post
title: 算法笔记（十八）
description: "学习资料：algorithm (Princeton University)"
keywords: test
category: Learning
tags: [algorithm]
---

## Maximum Flow
### Introduction
1. mincut problem
    * st-cut：给定一个edge-weighted digraph（其中每条边都有一个正的容量），source vertex s，target vertex t。则st-cut是指将所有点分成不相交的两部分，其中s在集合A中，t在集合B中。  
    * capacity：容量是指从A到B的边所有容量之和。  
    * minimum st-cut (mincut) problem：找到具有最小容量的cut。
2. maxflow problem
    * input：给定一个edge-weighted digraph（其中每条边都有一个正的容量），source vertex s，target vertex t。
    * st-flow：是指为每条边赋值方案使得以下两个条件成立，一是capacity constraint，即$0\le$edge's flow$\le$edge's capaciity；二是local equilibrium，即对于每个点都有inflow=outflow（除了s和t）。
    * value：一个flow的值是指在点t处的inflow。
    * maximum st-flow (maxflow) problem：找到一个具有最大value的flow。

### Ford-Fulkerson Algorithm
1. 初始化  
    将所有边的flow初始化为0。
2. 沿着augmenting path增加flow
    augmenting path
    找到一条从s到t的undirected path，使得以下条件成立：
    * 在向前的edge（非满）可以增加flow；
    * 在往后的edge（非空）可以减少flow。
3. 终止条件
    所有从s到t的路径满足其中一个条件（即没有augmenting path）：
    * 向前的edge是满的；
    * 向后的边是空的。
4. Ford-Fulkerson algorithm

    ```java
    Start with 0 flow.
    While there exists an augmenting path:
      - find an augmenting path
      - compute bottlenect capacity
      - increase flow on that path by bottleneck capacity
    ```
5. 需要解决的问题
    * 怎样计算mincut？
    * 怎样找到augmenting path？
    * 如果FF终止，是否计算出了maxflow？
    * FF总能够终止吗？如果是，在多少augmenting path之后会终止？

### Maxflow-Mincut theorem
1. flow和cut的关系
    * net flow：通过cut(A, B)的net flow是指从A到B的边的flow总和减去从B到A的边的flow总和。
    * flow-value lemma：记f为任意flow，(A, B)为任意cut。则通过(A, B)的net flow和f的value相等。
    * weak duality：记f为任意flow，(A, B)为任意cut。则flow的value$\le$cut的capacity。  
    证明：flow f的value=通过cut(A, B)的net flow$\le$cut(A, B)的capacity。
2. maxflow-mincut theorem
    * augmenting path theorem：flow f是一个maxflow当且仅当不存在augmenting path。
    * maxflow-mincut theorem：maxflow的value=mincut的capacity。
    * 证明：对于任意flow f,以下三个说法是等价的。
        * 存在一个cut使得其capacity和flow f的value相等；
        * f是一个maxflow；
        * f中没有augmenting path。
3. 从maxflow f中计算一个mincut (A, B)
    * 根据augmenting path theorem，f中不存在augmenting path；
    * 则计算出A，使得通过undirected path与s相连的点没有满的往前的边或空的往后的边。

### Analysis of Running Time
1. 在 Ford-Fulkerson algorithm部分提出的问题该如何解决？
    * 怎样计算mincut？（easy）
    * 怎样找到augmenting path？（BFS）
    * 如果FF终止，是否计算出了maxflow？（yes）
    * FF总能够终止吗？如果是，在多少augmenting path之后会终止？（若每条边的capacity都是整数，则FF总会终止；第二个问题仍需要分析）
2. Ford-Fulkerson algorithm with integer capacities
    * important special case：当capacity是在1到U之间的整数。
    * 不变性：通过Ford-Fulkerson计算的flow的value为整数。  
    证明：bottleneck capacity是一个整数；每条边的flow都是以bottle neck进行增加或减少的。
    * augmentation的数量$\le$maxflow的value。  
    证明：每次augmentation至少会增加1。
    * integrality theorem：存在一个value为整数的maxflow。（对于某些应用来说非常重要）  
    证明：当Ford-Fulkerson终止时，则找到了maxflow，且其value为整数。
    * bad case：即使每条边的capacity都是整数，仍会出现augmenting path的数量和maxflow的value相等。但这种情况可以很简单地避免。
3. 如何选择augmenting path？  
    对于一个含有V个点、E条边的digraph，且capacity为从1到U的整数，不同实现方法的运行时间不同。
    |augmenting path|number of paths|implementation|
    |:-:|:-:|:-:|
    |random path|$\le EU$|randomized queue|
    |DFS path|$\le EU$|stack (DFS)|
    |shortest path|$\le\frac{1}{2} EV$|queue(BFS)|
    |fattest path|$\le E\ln(EU)$|priority queue|

### Java Implementation
1. flow network representation
    * flow edge data type：将$e=v\to w$的flow $f_e$和capacity $c_e$联系起来。
    * flow network data type：可对边$e=v\to w$进行双向处理，则需要把e同时放入v和w的adjacency list中。
    * residual (spare) capacity
        * forward edge: residual capacity=$c_e-f_e$;
        * backward edge: residual capacity=$f_e$.
    * augment flow
        * forward edge: add $\Delta$;
        * backward edge: substract $\Delta$.
    * residual network：将原来的flow network变成双向的network。
2. flow edge
    * API
    ```java
    public class Flow Edge
        FlowEdge(int v, int w, double capacity)
        int from()
        int to()
        int other(int v)
        double capacity()
        double flow()
        double residualCapacityTo(int v)
        void addResidualFlowTo(int v, double delta)
    ```
    * java implementation
    ```java
    public class FlowEdge
    {
        private final int v, w;
        private final double capacity;
        private double flow;
        public FloeEdge(int v, int w, double capacity)
        {
            this.v = v;
            this.w = w;
            this.capacity = capacity;
        }
        public int from() { return v; }
        public int to() { return w; }
        public double capacity() { return capacity; }
        public double flow() { return flow; }
        public int other(int vertex)
        {
            if (vertex == v) return w;
            else if (vertex == w) return v;
            //else throw new IllegalArgumentException();
        }
        public double residualCapacity(int vertex)
        {
            if (vertex == v) return flow;
            else if (vertex == w) return capacity - flow;
            //else throw new IllegalArgumentException();
        }
        public void addResidualFlowTo(int vertex, double delta)
        {
            if (vertex == v) flow -= delta;
            else if (vertex == w) flow += delta;
            //else throw new IllegalArgumentException();
        } 
    }
    ```
3. flow network
    * API
    ```java
    public class FlowNetwork
        FlowNetwork(int V)
        void addEdge(FlowEdge e)
        Iterable<FlowEdge> adj(int v)
        //optional
        FlowNetwork(In in)
        Iterable<FlowEdge> edges()
        int V()
        int E()
        String toString()
    ```
    * java implementation
    ```java
    public class FloNetwork
    {
        private final int V;
        private Bag<FlowEdge>[] adj;
        public FlowNetwork(int V)
        {
            this.V = V;
            adj = (Bag<FlowEdge>()) new Bag[V];
            for (int v = 0; v < V; v++)
                adj[v] = new Bag<FlowEdge>();
        }
        public void addEdge(FlowEdge e)
        {
            int v = e.from();
            int w = e.to();
            adj[v].add(e);
            adj[w].add(e);
        }
        public Iterable<FlowEdge> adj(int v)
        { return adj[v]; }
    }
    ```
4. 找到最短的augmenting path (BFS)
    ```java
    private boolean hasAugmentingPath(FlowNetwork G, int s, int t)
    {
        edgeTo = new FlowEdge[G.V()];
        marked = new boolean[G.V()];
        Queue<Integer> queue = new Queue<Integer>();
        queue.enqueue(s);
        marked[s] = true;
        while (!queue.isEmpty())
        {
            int v = queue.dequeue;
            for (FlowEdge e : G.adj(v))
            {
                int w = e.other(V);
                if (!marked[w] && e.residualCapacity(w) > 0)
                {
                    edgeTo(w) = e;
                    marked[w] = true;
                    queue.enqueue(w);
                }
            }
        }
        return marked[t];
    }
    ```
5. Ford-Fulkerson algorithm
    ```java
    public class FordFulkerson
    {
        private boolean[] marked;
        private FlowEdge[] edgeTo;
        private double value;
        public FordFulkerson(FlowNetwork G, int s, int t)
        {
            value = 0.0;
            while (hasAugmentingPath(G, s, t))
            {
                double bootle = Double.POSITIVE_INFINITY;
                for (int v = t; v != s; v = edgeTo[v].other(v))
                    bottle = Math.min(bottle, edgeTo[v].residualCapacityTo(v));
                for (int v = t; v != s; v = edgeTo[v].other(v))
                    edgeTo[v].addResidualFlowTo(v, bottle);
                value += bottle;
            }
        }
        private boolean hasAugmentingPath(FlowNetwork G, int s, int t)
        { /* as previous */ }
        public double value()
        { return value; }
        public boolean inCut(int v)
        { return marked[v]; }
    }
    ```

### Applications
1. 应用举例
    * bipartite matching problem
    * baseball elimination problem
2. 总结
    * mincut problem：找到一个具有最小capacity的st-cut
    * maxflow problem：找到一个具有最大value的st-flow
    * duality：maxflow的value = mincut的capacity
    * 目前较成功的方法：Ford-Fulkerson、Preflow-push
    * 挑战：实现在实际应用中的线性时间的mincut/maxflow问题；理论上证明最坏情况下可实现线性时间。
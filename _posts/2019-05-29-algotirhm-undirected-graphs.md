---
layout: post
title: 算法笔记（十四）
description: "学习资料：algorithm (Princeton University)"
keywords: test
category: Learning
tags: [algorithm]
---

## Undirected Graphs
### Graph API
1. graph API
    ```java
    public class Graph
        Graph(int V) //创建含V个点的空图
        Graph(In in) //通过输入创建图
        void addEdge(int v, int w) //连接v和w，形成边
        Iterable<Integer> adj(int v) //与v相邻的点
        int V() //点个数
        int E() //边个数
    ```
2. sample client
    ```java
    //打印出所有相连的点
    In in = new In(args[0]);
    Graph G = new Graph(in);
    for (int v = 0; v < G.V(); v++)
        for (int w : G.adj(v))
        StdOut.println(v + "-" + w);
    //返回与v相连的点个数degree
    public static int degree(Graph G, int v)
    {
        int degree = 0;
        for (int w : G.adj(v))
            degree++;
        return degree;
    }
    ```
3. 图的表示方法
    * 维护边（由两个点组成）的列表
    * 维护一个$V\times V$的二维boolean数组，若v与w相连，则该元素的值为1，否则为0。
    * 最常见的做法是维护一个以点为索引的数组，其中每个元素都有一个列表来存放与其相连的点。所需时间和空间都与元素个数成正比。
    * 各表示方法的总结  
    实际应用中的图通常是稀疏的，也就是有很多点，但平均的边个数较少。因此，第三种方法所需时间和空间都较少。

    |representation|space|add edge|edge between v and w|iterate over tertices adjacent to v|
    |:-:|:-:|:-:|:-:|:-:|
    |list of edges|$E$|$1$|$E$|$E$|
    |adjacency matrix|$V^2$|$1$|$1$|$V$|
    |adjacency lists|$E+V$|$1$|degree(v)|degree(v)|
4. adjacency-list graph的java实现
    ```java
    public class Graph
    {
        //使用bag数据结构表示adjacency lists
        private final int V;
        private Bag<Integer>[] adj;
        //创建含有V个点的图
        public Graph(int V)
        {
            this.V = V;
            adj = (Bag<Integer>[]) new Bag[V];
            for (int v = 0; v < V; v++)
                adj[v] = new Bag<Integer>(); 
        }
        //添加v-w边
        public void addEdge(int v, int w)
        {
            adj[v].add(w);
            adj[w].add(v);
        }
        //与v相连节点的迭代器
        public Iterable<Integer> adj(int v)
            { return adj[v]; }
    }
    ```

### Depth-First Search
1. 迷宫问题
    * 创建一个图来解决迷宫问题，其中点表示交叉路口，边表示通道。
    * Tremaux maze exploration  
    准备一个带绳子的球，每遇到一个路口和通道就进行标记，当没有未访问的选择时，逐步向后退。
2. 深度优先搜索
    * 目标：系统地遍历图中的所有路径  
    做法：模仿迷宫问题的解决方法
    应用：找到与给定起点相连的所有顶点；找到两个点之间的一条路径。
    ```java
    DFS(to visit a vertex v)
        Marked v as visited.
        Recursively visit all unmarked vertices w adjacent to v.
    ```
    * 设计模式：将图这种数据结构从图形处理中分离出来。
        * 创建对象Graph；
        * 将Graph对象传递到一个图形处理程序中；
        * 查询图形处理程序以得到信息。
    * API
    ```java
    public class Paths
        Paths(Graph G, int s)// 在Graph中找到起点s的所有路径
        boolean hasPathTo(int v)// 从s到v是否存在路径
        Iterable<Integer> pathTo(int v)// 从s到v的路径，若没有则为空
    ```
    * client的应用
    ```java
    //打印出所有与s相连的点
    Paths paths = new Paths(G, s);
    for (int v = 0; v < G.V(); v++)
        if (paths.hasPathTo(v))
            StdOut.println(v);  
    ```
    * 数据结构
        * 用boolean数组marked[]来标记访问过的点。
        * 用整数数组edgeTo[]来保存路径的轨迹，(egdeTo[w] == v)表示通过边v-w第一次访问w。
        * 用于递归的函数调用栈。
    * java实现
    ```java
    public class DepthFirstPaths
    {
        private boolean[] marked;
        private int[] edgeTo;
        private int s;
        public DepthFirstPaths(Graph G, int s)
        {
            ...
            dfs(G, s);
            ...
        }
        private void dfs(Graph G, int s)
        {
            marked[v] = true;
            for (int w : G.adj(v))
                if (!marked[w])
                {
                    dfs(G, w);
                    edgeTo[w] = v;
                }
        }
    }
    ```
    * 性质  
        * DFS将与s相连的所有点标记起来所需时间与它们的degree之和（加上初始化marked[]数组的时间）成正比。
        * 经过DFS之后，可以在常数时间里检验点v是否与点s相连（通过marked[]实现），可以在与其长度成正比的时间里找到v-s的路径（若存在，通过edgeTo[]实现）。
    * 经过DFS后上述两个操作的实现
    ```java
    public boolean hasPathTo(int v)
    { return marked[v]; }
    public Iterable<Integer> pathTo(int v)
    {
        if (!hasPathTo(v)) return null;
        Stack<Integer> path = new Stack<Integer>();
        for (int x = v; x != s; x = edgeTo[x])
            path.push(x);
        path.push(s);
        return path;
    ```

### Breadth-First Search
1. 基本思路
    ```java
    BFS(from source vertex s)
        Put s onto a FIFO queue, and mark s as visited.
        Repeat until the queue is empty:
         - remove the least recently added vertex v
         - addd each of v's unvisited neighbors to the queue, and mark them as visited.
    ```
2. java实现
    ```java
    public class BreadthFirstSearch
    {
        private boolean[] marked;
        private int[] edgeTo;
        private int[] distTo;
        ...
        private void bfs(Graph G, int s)
        {
            Queue<Integer> q = new Queue<Integer>();
            q.enqueue(s);
            marked[s] = true;
            distTo[s] = 0;
            while (!q.isEmpty())
            {
                int v = q.dequeue;
                for (int w : G.adj(v))
                {
                    if (!marked[])
                    {
                        q.enqueue(w);
                        marked[w] = true;
                        edgeTo[w] = v;
                        distTo[w] = dist[v] + 1;
                    }
                }
            }
        }
    }
    ```
3. 性质
    * 在任意相连的图G中，BFS可在与$E+V$成正比的时间里找到从s到所有点的最短路径。
    * 因此。BFS常用于寻找最短路径的问题上。

### Connected Components
1. 连接查询
    * 定义：点v和w之间是否有路径相连
    * API
    ```java
    public class CC
        CC(Graph G)//站到G中的连通分量
        boolean connected(int v, int w)//v与w是否相连？
        int count()//连接分量的个数
        int id(int v)//v的分量标识
2. 连通分量
    * “连接”关系是一个等价关系：
        * 自反性：v与v是相连的；
        * 对称性：若v与w相连，则w与v也相连；
        * 传递性：若v与w相连，w与x相连，则v与x相连。
    * 连通分量是相连点的最大集合。给定连通分量，可在常数时间内回答是否连通的问题。
    * 将点分成连通分量
    ```java
    Connected components
        Initialize all vertices v as unmarked.
        For each unmarked vertex v, run DFS to identify all vertices discovered as part of the same component.
    ```
3. 用DFS找连通分量的java实现
    ```java
    public class CC
    {
        private boolean[] marked;
        private int[] id;
        private int count;
        public CC(graph G)
        {
            marked = new boolean[G.V()];
            id = new int[G.V()];
            for (int v = 0; v < G.V(); v++)
            {
                if (!marked[v])
                {
                    dfs(G, v);
                    count++;
                }
            }
        }
        public int count();
        { return count; }
        public int id(int v);
        { return id[v]; }
        public boolean connected(int v, int w)
        { return id[v] == id[w]; }
        private void dfs(Grapf G, int v)
        {
            marked[v] = true;
            id[v] = count;
            for (int w : G.adj(v))
                if (!marked[w])
                    dfs(G, w);
        }
    }
    ```

### Challenges

|problem|BFS|DFS|time|
|:-:|:-:|:-:|:-:|
|path between s and t|$\surd$|$\surd$|$E+V$|
|shortest path between s and t|$\surd$||$E+V$|
|connected components|$\surd$|$\surd$|$E+V$|
|biconnected components||$\surd$|$E+V$|
|cycle|$\surd$|$\surd$|$E+V$|
|Euler cycle||$\surd$|$E+V$|
|Hamilton cycle|||$2^{1.657V}$|
|bipartiteness|$\surd$|$\surd$|$E+V$|
|planarity||$\surd$|$E+V$|
|gragh isomorphism|||$2^{c\sqrt{V\log V}}$|
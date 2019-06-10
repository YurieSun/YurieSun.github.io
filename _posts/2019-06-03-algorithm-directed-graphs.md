---
layout: post
title: 算法笔记（十五）
description: "学习资料：algorithm (Princeton University)"
keywords: test
category: Learning
tags: [algorithm]
---

## Directed Graphs
### Digraph API
1. diagraph API
    ```java
    public class Digraph
        Diagraph(int v) //创建一个含V个vertices的digraph
        Diagraph(In in) //从输入流中创建digraph
        void addEdge(int v, intw) //添加从v指向w的有向边
        Iterable<Integer> adj(int v) //以v为起点所指向的点
        int V() //vertices的数量
        int E() //edge的数量
        Digraph reverse() //digraph的反向
        String toString() //字符的表达
    ```
2. adjacency-lists digraph representation
    与undirected graph相比只有addEdge()不一样
    ```java
    public class Digraph
    {
        private final int V;
        private final Bag<Integer>[] adj;
        //给每个vertex创建空的Bag
        public Digraph(int V)
        {
            this.V = V;
            adj = (Bag<Integer>[]) new Bag[V];
            for (int v = 0; v < V; v++)
                adj[v] = new Bag<Integer>();
        }
        public void addEdge(int v, int w)
        {
            adj[v].add(w);
        }
        public Iterable<Integer> adj(int v)
        { return adj[v]; }
    }
    ```
3. 各种digraph表示方法的比较

    |representation|space|insert edge from v to w|edge from v to w?|iterate over vertices pointing from v?|
    |:-:|:-:|:-:|:-:|:-:|
    |list of edges|$E$|$1$|$E$|$E$|
    |adjacency matrix|$V^2$|$1$|$1$|$V$|
    |adjacency lists|$E+V$|$1$|outdegree(v)|outdegree(v)|

### Digraph Search
1. diagraph的DFS
    * 基本思路（与undirected graph一样）  
    每个undirected graph都是一个digraph（有两个方向的边），因此DFS是一个diagraph算法。
    ```java
    DFS(to visit a vertex v)
        Mark v as visited
        Recursively visit all unmarked vertices w pointing from v
    ```
    * java实现  
    与undirected graph代码完全相同，仅将类名或变量名Graph改为Digraph。
    ```java
    public class DirectedDFS
    {
        private boolean[] marked;
        public DirectedDFS(Digraph G, int s)
        {
            marked = new boolean[G.V()];
            dfs(G, s);
        }
        private void dfs(Digraph G, int v)
        {
            marked = true;
            for (int w : G.adj(v))
                if (!marked[w]) dfs(G, w);
        }
        public boolean visited(int v)
        { return marked[v]; }
    }
    ```
    * reachability applications
        * program control-flow analysis  
        每一个程序看成一个digraph，将每一个基本指令（直接相连的程序）看成vertex，将程序间的跳转看成edge。则可找到（并删除）不可到达的程序，即dead-code elimination；可以决定出口是否能到达，即infinite-loop detection。
        * mark-sweep garbage collector  
        每一个数据结构看成digraph，将对象看成vertex，将引用看成edge。并将程序可直接获得的对象作为roots。将程序间接获得的对象（从roots出发可到达的对象）作为reachable objects。mark-sweep算法将所有可到达的对象进行标记，若对象未被标记，则视为garbage被回收。其中的内存花销仅为，在每个对象多加一个mark位（并加上DFS栈的内存）。
    * 总结
        * DFS可解决简单的digraph问题，如reachability、path finding、topological sort、directed cycle detection。
        * DFS是解决复杂digraph问题的基础，如2-satisfiability、directed Euler path、strongly-connected components。
2. digraph 的BFS
    * 基本思路（与undirected graph一样）  
    每个undirected graph都是一个digraph（有两个方向的边），因此BFS是一个diagraph算法。

    ```java
    BFS(from source vertex s)
        Put s onto a FIFO queue, and mark s as visited.
        Repeat until the queue is empty:
         - remove the least recently added vertex v
         - for each unmarked vertex pointing from v: add to queue and mark as visited.
    ```
    * 应用
        * multi-source shortest paths  
        目标：若给定一个digraph和一个含多个source vertex的集合，找到从该集合中任一点到其它点的最短路径。  
        方法：使用BFS，但初始化时将所有source vertex入队。
        * web crawler  
        目标：从某些root网页开始进行网络抓取。  
        方法：选择某些网页作为root，维护要找的网站的queue，维护找到网站的set，将下一网站出队，并将其连接的网站入队。若使用DFS，则会沿着某个网站一直深度搜索下去，与网站搜索想要的功能不太相符。  
        ```java
        Queue<String> queue = new Queue<String>();
        SET<String> marked = new SET<String>();
        String root = "http://www.princeton.edu";
        queue.enqueue(root);
        marked.add(root);
        while (!queue.isEmpty())
        {
            // 从队列的下一网站中读入新的网站
            String v = queue.dequeue();
            StdOut.println(v);
            In in = new In(v);
            String input = in.readAll();
            // 使用正则表达式找到形为http://xxx.yyy.zzz的网站
            String regexp = "http://(\\w+\\.)+(\\w+)";
            Pattern pattern = Pattern.compile(regexp);
            Matcher matcher = pattern.matcher(input);
            while (matcher.find())
            {
                String w = matcher.group();
                if (!marked.contains(w))
                {
                    marked.add(w);
                    queue.enqueue(w);
                }
            }
        ```

### Topological Sort
1. 基本定义
    * DAG：Directed acyclic graph
    * topological sort：重建DAG使得所有边的方向指向上端。
    * 方法：DFS  
    运行DFS，再以reverse postorder的顺序返回点。
2. java实现
    ```java
    public class DepthFirstOrder
    {
        private boolean[] marked;
        private Stack<Integer> reversePostorder;
        public DepthFirstOrder(Diagraph G)
        {
            reversePostorder = new Stack<Integer>();
            marked = new boolean[G.V()];
            for (int v = 0; v < G.V(); v++)
                if (!marked[v] dfs(G, v);
        }
        private void dfs(Digraph G, int v)
        {
            marked[v] = true;
            for (int w : adj(v))
                if (!marked[w]) dfs(G, w);
            reversePostorder.push(v);
        }
        public Iterable<Integer> reversePostorder()
        { return reversePostorder; }
    }
    ```
3. 算法的正确性说明
    * DAG的reverse DFS postorder是一个topological ordeer。
    * 考虑任一从v指向w的边，当调用dfs(v)时，有以下三种情况：
        * case 1：dfs(w)已经被调用并返回，那么w在v之前被处理；
        * case 2：dfs(w)未被调用，dfs(w)将被dfs(v)直接或间接调用，并将在dfs(v)之前完成返回，因此w在v之前被处理；
        * case3：dfs(w)已经被调用但未返回，这在DAG中不可能发生。若存在这种情况，则函数调用栈中存在w指向v的路径，说明v至w有一个回路，这违背了DAG的定义。
4. directed cycle detection
    * 当且仅当diagragh中没有回路时，它才有一个topological order。
    * 若有回路，则不可能有topological order；若没有回路，基于DFS的算法可以找到一个topological order。
    * 可用于检测digraph中是否含有回路。
    * 应用
        * 计划制定：有回路说明不能制定一个这样的计划。
        * java compiler中的cyclic inheritance：若函数调用存在回路，则报错。
        * 电子表格的计算：Excel中使用函数时，若存在回路，则出错。
5. DFS的各种顺序
    * preorder：dfs()调用的顺序
    * postorder：dfs()返回的顺序
    * reverse postorder：dfs()返回的反顺序
    ```java
    private void dfs(Graph G, int v)
    {
        marked[v] = true;
        preorder.enqueue(v);
        for (int w : G.adj(v))
            if (!marked[w]) dfs(G, w);
        postorder.enqueue(v);
        reversePostorder.push(v);
    }
    ```

### Strong Components
1. strong-connetced components
    * 定义  
    若点v和w同时存在v到w的有向路径和w到v的有向路径，则称v和w是strongly connected的。
    * 性质
        * 自反性：v与本身是强连接的；
        * 对称性：若v与w是强连接的，则w与v也是强力连接的；
        * 传递性：若v与w是强连接的，w与x是强连接的，则v与x是强连接的。
    * strong component是指一个最大的包含所有强连接点的集合。
    * 应用
        * 生物链：vertex是生物种类，edge是从生产者到消费者的路径，则strong component指具有同一能量流的生物种类的集合。
        * 软件模块：vertex是软件模块，edge是从某一模块到所依赖的模块，则strong component指有相互作用的模块集合。由此可将strong component的模块进行封装，以改善设计。
2. Kosaraju-Sharir算法
    * $G$中的strong component与$G^R$中的一样。
    * kernel DAG：将每个strong component视为单个点。
    * 实现想法：计算kernal DAG的topological order，再以reverse topological order的顺序运行DFS。
    * 实现
        * 第一步：计算$G^R$中的strong component；
        * 第二步：在$G$中运行DFS，以$G^R$中reverse postorder的顺序访问$G$中unmarked的点。
    * 数学分析：该算法计算digraph中的strong component的时间与$E+V$成正比。
    * java实现
    ```java
    public class KosarajuSharirSCC
    {
        private boolean marked[];
        private int[] id;
        private int count;
        public KosarajuSharirSCC(Digraph G)
        {
            marked = new boolean[G.V()];
            id = new int[G.V()];
            DepthFirstOrder dfs = new DepthFirstOrder(G.reverse());
            for (int v : dfs.reversePostorder())
            {
                if (!marked[v])
                {
                    dfs(G, v);
                    count++;
                }
            }
        }
        private void dfs(Digraph G, int v)
        {
            marked[v] = true;
            id[v] = count;
            for (int w :G.adj(v))
                if(!marked[w])
                    dfs(G, w);
        }
        public boolean stronglyConneted(int v, int w)
        { return id[v] == id[w]; }
    }
    ```
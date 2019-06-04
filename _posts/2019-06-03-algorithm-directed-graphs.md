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

### Strong Components
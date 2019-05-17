---
layout: post
title: 算法笔记（十）
description: "学习资料：algorithm (Princeton University)"
keywords: test
category: Learning
tags: [algorithm]
---

## Balanced Search Trees
### 2-3 Search Trees
1. 基本结构
    * 含有两种节点：2-node，一个键，两个子节点；3-node，两个键，三个子节点
    * 顺序遍历可以得到有序的数列。
    * 2-3 tree有着完美的平衡，即所有从根节点到空链接的路径长度均相同。
2. 基本操作
    * search：从根节点开始，比较节点中的键值与要找的键值，直至找到。
    * insert
        * 在底部将新的键插入2-node创建3-node
        * 在底部将新的键插入3-node：先插入形成一个临时的4-node，将4-node的中间键移入父节点，若又形成4-node，重复操作，直至形成规范的2-3 tree。
3. 性质
    * 将4-node分裂仅涉及局部的变换，与其他节点无关，因此操作次数少，更加高效。
    * 在2-3 tree的构建与变换中，始终能保持对称性与完美的平衡，这是由于每次变换均能保持该性质。
    * 树的高度
        * 最坏情况：$\lg N$（所有节点均为2-node）
        * 最好情况：$\log_3 N\sim0.631\lg N$（所有节点均为3-node）
        * 含百万节点的树高在12和20之间，含十亿节点的树高在18和30之间。
        * 因此，2-3 tree可以保证在最坏情况下search和insert操作为对数相关。
4. ST实现的总结
    <table>
    <tr>
        <td rowspan="2">ST implementation</td>
        <td colspan="3">worst-case cost<br>(after N inserts)</td>
        <td colspan="3">average case<br>(after N random inserts)</td>
        <td rowspan="2">ordered ops?</td>
        <td rowspan="2">operations on keys</td>
    </tr>
    <tr>
        <td>search</td>
        <td>insert</td>
        <td>delete</td>
        <td>search hit</td>
        <td>insert</td>
        <td>delete</td>
    </tr>
    <tr>
        <td>sequential search<br>(unordered list)</td>
        <td>N</td>
        <td>N</td>
        <td>N</td>
        <td>N/2</td>
        <td>N</td>
        <td>N/2</td>
        <td>no</td>
        <td>equals()</td>
    </tr>
    <tr>
        <td>binary search<br>(ordered array)</td>
        <td>lg N</td>
        <td>N</td>
        <td>N</td>
        <td>lg N</td>
        <td>N/2</td>
        <td>N/2</td>
        <td>yes</td>
        <td>compareTo()</td>
    </tr>
    <tr>
        <td>BST</td>
        <td>N</td>
        <td>N</td>
        <td>N</td>
        <td>1.39 lg N</td>
        <td>1.39 lg N</td>
        <td>sqrt(N)</td>
        <td>yes</td>
        <td>compareTo()</td>
    </tr>
    <tr>
        <td>2-3 tree</td>
        <td>c lg N</td>
        <td>c lg N</td>
        <td>c lg N</td>
        <td>c lg N</td>
        <td>c lg N</td>
        <td>c lg N</td>
        <td>yes</td>
        <td>compareTo()</td>
    </tr>
    </table>

    * 其中，2-3 tree中的c与实现方式有关。而2-3 tree的直接实现较困难，下一节将介绍简洁的实现方法。

### Red-Black BSTs
1. left-leaning red-black BSTs
    * 将2-3 tree表示成BST，对于3-node，使用内部左倾链接起来。
    * 因此，BST的等效定义为：没有任何一个节点有两个红链接，所有从根节点到空链接的路径有相同数量的黑链接，红链接向左倾。
    * 由此得到的LLRB与2-3 tree的操作是一一对应的。
2. search的实现
    * 若忽略颜色，search操作与基本的BST是一样的，但由于更好的平衡性，会更高效。
    ```java
    public val search(Key key)
    {
        Node x = root;
        while (x != null)
        {
            int cmp = key.compareTo(x.key);
            if (cmp < 0) x = x.left;
            else if (cmp > 0) x = x.right;
            else return x.val;
        }
        return null;
    }
    ```
    * 大部分操作（如floor、iteration、selection等）均与基本的BST相同。
3. red-black BST的表示
    ```java
    private static final boolean RED = true;
    private static final boolean BLACK = false;
    private class Node 
    {
        Key key;
        Value val;
        Node left, right;
        boolean color; //增加表示颜色的属性
    }
    private boolean isRed(Node x)
    {
        if (x == null) return false; //null links are black
        return x.color == RED; 
    }
    ```
4. red-black BST的基本操作
    * left rotation：将右倾的red link变成左倾。
    ```java
    private Node rotateLeft(Node h)
    {
        assert isRed(h.right);
        Node x = h.right;
        h.right = x.left;
        x.left = h;
        x.color = h.color;
        h.color = RED;
        return x;
    }
    ```
    * right rotation：将左倾的red link变成右倾。（与left rotation完全对称）
    ```java
    private Node rotateRight(Node h)
    {
        assert isRed(h.left);
        Node x = h.left;
        h.left = x.right;
        x.right = h;
        x.color = h.color;
        h.color = RED;
        return x;
    }
    ```
    * color flip：将左右link均为red的节点颜色上移，即分裂（临时）4-node。
    ```java
    private void flipColors(Node h)
    {
        assert !isRed(h);
        assert isRed(h.left);
        assert isRed(h.right);
        h.color = RED;
        h.left.color = BLACK;
        h.right.color = BLACK;
    }
    ```
5. LLRB tree的插入操作
    * 将元素插入正确的空链接处，再通过上述的基本操作保持树的对称性与平衡性。分析之后可知，插入中的较复杂情况均可逐步简化为固定的简单情况。因此，所有的情况最终可总结为以下情况：
        * 右节点为red，左节点为black：rotate left
        * 左节点为red，左节点的子节点也为red：rotate right
        * 左右节点均为red：flip colors
    ```java
    private Node put(Node h, Key key, Value val)
    {
        if (h == null) return new Node(key, val, RED);
        int cmp = key.compareTo(h.key);
        if (cmp < 0) h.left = put(h.left, key, val);
        else if (cmp > 0) h.right = put(h.right, key, val);
        else h.val = val;
        //插入之后进行调整，以下判断不可调整顺序
        if (isRed(h.right) && !isRed(h.left)) rotateLeft(h);
        if (isRed(h.left) && isRed(h.left.left)) rotateRight(h);
        if (isRed(h.left) && isRed(h.right)) flipColors(h);

        return h;
    }
    ```
6. LLRB trees的高度
    * 由于所有从根节点到空链接的路径有相同数量的黑链接，且在同一层不会有两个红链接，因此在最坏情况下，树的高度$\le2\lg N$。
    * 在通常情况下，树的高度为$\sim1.0\lg N$。
7. ST实现的总结
    <table>
    <tr>
        <td rowspan="2">ST implementation</td>
        <td colspan="3">guarantee</td>
        <td colspan="3">average case<br></td>
        <td rowspan="2">ordered ops?</td>
        <td rowspan="2">key interface</td>
    </tr>
    <tr>
        <td>search</td>
        <td>insert</td>
        <td>delete</td>
        <td>search hit</td>
        <td>insert</td>
        <td>delete</td>
    </tr>
    <tr>
        <td>sequential search<br>(unordered list)</td>
        <td>N</td>
        <td>N</td>
        <td>N</td>
        <td>N/2</td>
        <td>N</td>
        <td>N/2</td>
        <td>no</td>
        <td>equals()</td>
    </tr>
    <tr>
        <td>binary search<br>(ordered array)</td>
        <td>lg N</td>
        <td>N</td>
        <td>N</td>
        <td>lg N</td>
        <td>N/2</td>
        <td>N/2</td>
        <td>yes</td>
        <td>compareTo()</td>
    </tr>
    <tr>
        <td>BST</td>
        <td>N</td>
        <td>N</td>
        <td>N</td>
        <td>1.39 lg N</td>
        <td>1.39 lg N</td>
        <td>sqrt(N)</td>
        <td>yes</td>
        <td>compareTo()</td>
    </tr>
    <tr>
        <td>2-3 tree</td>
        <td>c lg N</td>
        <td>c lg N</td>
        <td>c lg N</td>
        <td>c lg N</td>
        <td>c lg N</td>
        <td>c lg N</td>
        <td>yes</td>
        <td>compareTo()</td>
    </tr>
    <tr>
        <td>red-black BST</td>
        <td>2 lg N</td>
        <td>2 lg N</td>
        <td>2 lg N</td>
        <td>1.0 lg N</td>
        <td>1.0 lg N</td>
        <td>1.0 lg N</td>
        <td>yes</td>
        <td>compareTo()</td>
    </tr>
    </table>

    * red-black BST平均情况下的操作中的具体系数是不知道的，但非常接近1。

### B-Trees
1. 文件系统模型
    * 在文件系统中，找到第一页所花的时间要比在某一页中找数据多，因此，需要实现能够高效地找到第一页从而找到数据的算法。
2. B-trees
    * 与这是2-3 tree类似。但在B-tree中，假设每个节点可存放的数目为$M$，根节点最少有2个键，其余节点最少有$\frac{M}{2}$个键。最底层节点（即external nodes）存放的是从客户端接收的键，其余上层节点（即internal nodes）存放的是每个子节点的最小键，从而进行搜索。  
    * search：从根节点开始，逐层向下，直到在external nodes中找到对应的键。
    * insert：从根节点开始逐层向下，自external nodes中找到该节点所在位置，并插入。若某节点存满，则逐层向上进行分裂。
3. B-trees的平衡性
    * 设每个节点可存放数目为$M$，键的总数目为$N$，则每次search或insert所需要花费仅在$\log_{M-1}N$和$\log_{\frac{M}{2}}N$之间。
4. balanced trees的应用
    * red-black trees广泛应用于 系统符号表中
        * Java: java.util.TreeMap, java.util.TreeSet
        * C++ STL: map, multimap, multiset
        * Linux kernel: completely fair scheduler, linux/rbtree.h
        * Emacs: conservative stack scaning
    * B tree有许多变种，如B+ tree、B* tree、B# tree，他们广泛应用于文件系统和数据库中。
        * Windows: NTFS
        * Mac: HFS, HFS+
        * Linux: ReiserFS, XFS, Ext3FS, JFS
        * Databases: ORACLE, DB2, INGRES, SQL, PostgreSQL
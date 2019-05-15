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

### B-Trees
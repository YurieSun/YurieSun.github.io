---
layout: post
title: 算法笔记（四）
description: "学习资料：algorithm (Princeton University)"
keywords: test
category: Learning
tags: [algorithm]
---

## 基本排序

### Rules of The Game
1. 回调

* 排序的目标是可以对任意类型的数据进行排序，为实现该目标，使用回调函数。

* 客户端将数组传给sort()函数，而sort()调用对象的compareTo()方法。

* 不同语言的回调实现  
java: interfaces  
C: function pointers  
C++: class-type functors  
C# :delegates  
Python, Perl, ML, Javascript: first-class functions

3. 回调的工作机制

* client

```java
import java.io.File;
public class FileSorter
{
    public static void main(String[] args)
    {
        File directory = new File(args[0]);
        File[] files = directory.listFiles();
        Insertion.sort(files);
        for (int i = 0; i < files.length; i++)
            StdOut.println(files[i].getName());
    }
}
```

* Comparable interface (built in to java)

```java
public interface Comparable<Item>
{
    public int compareTo(Item that);
}
```

* object implementation

```java
public class File
implements Comparable<File>
{
    ...
    public int compareTo(File b)
    {
        ...
        return -1;
        ...
        return +1;
        ...
        return 0;
    }
}
```

* sort implementation

```java
public static void sort(Comparable[] a)
{
    int N = a.length;
    for (int i = 0; i < N; i++)
        for (int j = i; j > 0; j--)
            if(a[j].compareTo(a[j-1]) < 0)
                exch(a, j, j-1);
            else break;
}
```

4. 全序关系（total order）

全序关系应满足：

* 反对称性：若$v\le w$且$w\le v$，则$v=w$。

* 传递性：若$v\le w$且$w\le x，则$v\le x。

* 完全性：v和w仅存在这三种关系，$v\le w$或$w\le v$或相等。

5. less与exchange的实现

* less implementation

```java
private static boolean less(Comparable v, Comparable w)
{
    return v.compareTo(w) < 0;
}
```

* exchange implementation

```java
private static void exch(Comparable[] a, int i, int j)
{
    Comparable swap = a[i];
    a[i] = a[j];
    a[j] = swap;
} 
```

### Selection Sort

1. 基本思想

将数组从左到右进行扫描，每次找出第i个元素右边的最小值，并与第i个交换，这样保证了第i个元素左边的均是以排序好的。

2. java实现

```java
public class Selection
{
    public static void sort(Comparable[] a)
    {
        int N = a.length;
        for (int i = 0; i < N; i++)
            int min = i;
            for (int j = i; j < N; j++)
                if (less(a[j], a[min]))
                    min = j;
            exch(a, i, min);
    }

    private static boolean less(Comparable v, Comparable w)
    { /* as before */ }
    public static void exch(Comparable[] a, int i, int j)
    { /* as before */ }
}
```

3. 数学分析

* 选择排序需$(N-1)+(N-2)+...+1+0$ ~ $\frac{N^2}{2}次比较与N次交换。

* 无论输入是什么样的（即使是有序的），仍然需要平方时间。

* 数据交换的次数是最少的，即N次。

### Insertion Sort

### Shellsort

### Shuffling

### Convex Hull
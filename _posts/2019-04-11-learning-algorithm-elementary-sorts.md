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
2. 回调的工作机制
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
3. 全序关系（total order） 
    全序关系应满足：  
    * 反对称性：若$v\le w$且$w\le v$，则$v=w$。  
    * 传递性：若$v\le w$且$w\le x$，则$v\le x$。  
    * 完全性：v和w仅存在这三种关系，$v\le w$或$w\le v$或相等。
4. less与exchange的实现
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
    * 选择排序需$(N-1)+(N-2)+...+1+0$ ~ $\frac{N^2}{2}$次比较与N次交换。
    * 无论输入是什么样的（即使是有序的），仍然需要平方时间。
    * 数据交换的次数是最少的，即N次。
### Insertion Sort
1. 基本思想  
将数组从左到右进行扫描，若第i个元素比左边的小，则交换位置，直到不再比左边的小停止。
2. java实现
    ```java
    public class Insertion
    {
        public static void sort(Comparable[] a)
        {
            int N = a.length;
            for (int i = 0; i < N; i++)
                for (int j = i; j > 0; j--)
                    if (less(a[j] <a[j-1])
                        exch(a, j, j-1);
                    else break;
        }

        private static boolean less(Comparable v, Comparable w)
        { /* as before */ }
        
        private static void exch(Comparable[] a, int i, int j)
        { /* as before */}
    }
    ```
3. 数学分析
    * 对于一个任意排序的数组，插入排序平均需 ~ $\frac{1}{4}N^2$次比较和 ~ $\frac{1}{4}N^2次交换。因此会比选择排序快一倍左右。
    * 最好情况：若数组是升序排列，则仅需$N-1$次比较和0次交换  
    最坏情况：若数组是降序排列，则需 ~ $\frac{1}{2}N^2$次比较和 ~ $\frac{1}{2}N^2次交换
    * 部分排序数组  
    定义：逆序对是一对乱序的元素对，而部分排序数组指逆序对的数量$\le cN$的数组。  
    性质：对于部分排序数列，插入排序的运行时间是线性的，因交换次数与逆序对次数相等，而比较次数等于交换次数$+(N-1)$。  
    因此对于部分有序数组，插入排序比选择排序的效率更高。
### Shellsort
1. 基本思想  
插入排序中每次只移动一个位置，考虑每次移动的位置为h，且h是一个递减的数列，从而使得数组逐步部分有序。
2. 插入算法的选择  
选择插入排序的原因是：对于h很大的情况，则每种排序算法的速度相近，而对于h很小的情况，数组已经基本有序了，对于部分有序的数组，插入排序速度更快。
3. h数列的选择  
较常用的为$3x+1$，而Sedgewick本人提出了一个性能较好的序列：1, 5, 19, 41, 109, 209, 505, 929, 2161, 3905, ...
4. java实现  
```java
public class Shell
{
    public static void sort(Comparavle[] a)
    {
        int N = a.length;

        int h = 1;
        while (h < N/3) h = 3*h + 1;

        while (h >=1)
        {
            for (int i = h; i < N; i++)
            {
                for (int j = i; j > h && less(a[j], a[j-h]); j -= h)
                    exch(a, j, j-h);
            }

            h = h/3;
        }
    }

    private static boolean less(Comparable v, Comparable w)
    { /* as before */ }
    private static void exch(Comparable[] a, int i, int j)
    { /* as before */ }
}
```
5. 数学分析  
最坏情况下，用$3x+1$的h数列的比较次数为$Oh(N^\frac{3}{2})。
6. 优点  
数组不是非常大时，速度很快；仅需很简短的代码就可以提高速度。
### Shuffling
1. 基本思想  
从左向右扫描数组，扫到第i个元素时，随机选择0到i中的一个整数r，交换a[i]和a[r]。这种方法是一种在线性时间里获得被均匀洗牌的数组。
2. java实现  
```java
public class StdRandom
{
    ...
    public static void shuffle(Object[] a)
    {
        int N = a.length;
        for (int i = 0; i < N; i++)
        {
            int r = StdRandom.uniform(i + 1);
            exch(a, i, r);
        }
    }
}
```
### Convex Hull
1. 定义  
凸包是指能将N个点包围起来的最小凸多边形。
2. 基本思想
选择y坐标最小的点记为p，其余各点按照与p的极角从小到大排列，按顺序扫描各点并连线，若产生了逆时针角度则取之，反之弃之。
3. 挑战  
    * 如何找到y坐标最小的p点？  
    定义一个全序，比较y坐标。
    * 如何根据与p的极角来排序？  
    定义一个全序
    * 如何知道p1到p2到p3是一个逆时针角？  
    计算几何学。
    * 如何高效排序？  
    mergesort可在$N\lg N$时间内完成排序。
    * 怎样解决多个点在一条直线上的情况？  
    需谨慎处理，但难度不大。
4. 判断逆时针角的实现  
定义一个有向面积
$$2*Area(a, b, c) = \\left{vmatrix}
a_x&a_y&1\\
b_x&b_y&1\\
c_x&c_y&1
\\end{vmatrix}=(b_x-a_x)(c_y-a_y)-(b_y-a_y)(c_x-a_x)$$
若有向面积大于0，则为逆时针角；若小于0，则为顺时针角；若等于0，则三点共线。
5. java实现点类  
```java
public class Point2D
{
    private final double x;
    private final double y;

    public Point2D(double x, double y)
    {
        this.x = x;
        this.y = y;
    }

    ...

    public static int ccw(Point2D a, Point2D b, Point2D c)
    {
        double area2 = (b.x-a.x)*(c.y-a.y) - (b.y-a.y)*(c.x-a.x);
        if (area2 < 0) return -1;
        else if(area2 > 0) return +1;
        else return 0;
    }
}
```

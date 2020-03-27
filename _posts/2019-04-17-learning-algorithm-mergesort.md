---
layout: post
title: 算法笔记（五）
description: "学习资料：algorithm (Princeton University)"
keywords: test
category: Learning
tags: [algorithm]
---

## Mergesort

### Mergesort
1. 两个经典排序算法
    * Mergesort  
    Java sort for objects.  
    Perl, C++ stable sort, Python stable sort, Firefox JavaScript, ...
    * Quicksort  
    Java sort for primitive types.  
    C qsort, Unix, Visual C++, Python, Matlab, Chrome JavaScript, ...
2. 基本思想  
    将数组分成两部分，每部分进行递归排序，将有序的两部分合并。
3. merge的java实现
    ```java
    private static merge(Comparable[] a, Comparable[] aux, int lo, int mid, int hi)
    {
        assert isSorted(a, lo, mid);
        assert isSorted(a, mid + 1, hi);

        for (int k = lo; k <= hi, k++)
            aux[k] = a[k];
        
        int i = lo, j = mid + 1;
        for (int k = lo; k <= hi; k++)
        {
            if (i > mid) a[k] = aux[j++];
            else if (j < hi) a[k] = aux[i++];
            else if (less(aux[j], aux[i])) a[k] = aux[j++];
            else a[k] = aux[i++];
        }

        assert isSorted(a, lo, hi);
    }
    ```
4. assertions
    * assertion用于检验程序中的假设，帮助检测错误，且能在重读程序时能快速知道起始的假设及最后做了什么。
    * java中的assertion接收一个返回值为boolean的函数，若返回值为false则报错。
    * 在程序中可随时开启assertion以达到不同目的。
    * 最好的使用方法是，在编程过程中使用assertion来检测错误，在最终的产品程序中禁用，若出现问题，可再次打开以找出错误。
5. mergesort的java实现
    ```java
    public class Merge
    {
        private static void merge(...)
        { /* as before */ }

        private static void sort(Comparable[] a, Comparable[] aux, int lo, int hi)
        {
            if (hi <= lo) return;
            int mid = lo + (hi - lo) / 2;
            sort(a, aux, lo, mid);
            sort(a, aux, mid + 1, hi);
            merge(a, aux, lo, mid, hi);
            //sort不断递归，将数组分为两个小数组，直到仅剩两个元素时，merge将这两个元素进行排序。
        }

        public static void sort(Comparable[] a)
        {
            aux = new Comparable[a.length];//在这里创建辅助数组，而不是上面的sort，反之会生成很多小数组，影响运行效率。
            sort(a, aux, 0, a.length - 1);
        }
    }
    ```
6. 数学分析
    * 对于有$N$个元素的数组，mergesort最多有$N\lg N$次比较和$6N\lg N$次数组访问。
    * 比较次数$C(N)$和数组访问次数$A(N)$分别满足：  
    $$\begin{cases}
        C(N)\le C([\frac{N}{2}])+C([\frac{N}{2}])+N & N>1 \\
        C(N)=0 & N=1
    \end{cases}$$  
    $$\begin{cases}
        A(N)\le A([\frac{N}{2}])+A([\frac{N}{2}])+6N &N>1 \\
        A(N)=0 & N=1
    \end{cases}$$
7. 内存分析
    * mergesort所使用额外的空间与$N$成正比，因为在最后一次归并时，所用到的辅助数组aux[]的长度为$N$。
    * 不需要辅助数组的排序算法（in-place）所使用的额外的空间$\le c\log N$，如insertion sort、selection sort、shellsort。
    * 目前没有实现mergesort的in-place算法。
8. 提高效率的方法
    * 对于小数组使用插入排序，因为mergesort的递归会使用很多开销，并产生很多小数组。则定义一个分段数，如Cutoff $\approx 7$。
    ```java
    private static sort(Comparable[] a, Comparable[] b, aux, int lo, int hi)
    {
        if (hi <= lo +CUTOFF - 1)
        {
            Insertion.sort(a, lo, hi);
            return;
        }
        int mid = (lo + hi)/2;
        sort(a, aux, lo, mid);
        sort(a, aux, mid + 1, hi);
        merge(a, aux, lo, mid, hi);
    }
    ```
    * 若数组已排序，则停止；即判断左半边的最大值是否小于右半边的最小值。
    ```java
    private static void sort(Comparable[] a, Comparable[] aux, int lo, int hi)
    {
        if (hi <= lo) return;
        int mid = (lo +hi)/2;
        sort(a, aux, lo, mid);
        sort(a, aux,mid + 1,hi);
        if (!less(a[mid+1], a[mid])) return; //增加的判断代码
        merge(a, aux, lo, mid, hi);
    }
    ```
    * 执行merge函数时，删除将数组从a[]复制到aux[]的过程（仅节约时间，不节约空间）。此法要在sort(a)中初始化aux[]并将a[]复制到aux[]。  
    ```java
    private static void merge(Comparable[] a, Comparable[] aux, int lo, int mid, int hi)
    {
        int i = lo, j = mid + 1;
        for (int k = 0; k <= hi; k++)
        {
            if (i > mid) aux[k] = a[j++];
            else if (j > hi) aux[k] = a[i++];
            else if (less(a[j], a[i])) aux[k] = a[j++];
            else aux[k] = a[i++];
        }
    }
    ```
    ```java
    private static void sort(Comparable[] a, Comparable[] aux, int lo, int hi)
    {
        if (hi <= lo) return;
        mid = (lo +hi)/2;
        sort(aux, a, lo, mid);
        sort(aux, a, mid + 1,hi);
        merge(a, aux, lo, mid, hi);
    }
    ```

### Bottom-Up Mergesort
1. 基本思想  
    将数组从长度为1、2、4、8、16的小数组逐步排序合并。
2. java实现
    ```java
    public class MergeBU
    {
        private static void merge(...)
        { /* as before */ }

        public static void sort(Comparable[] a)
        {
            int N = a.length;
            Comparable[] aux = new Comparable[N];
            for (int sz = 1; sz < N; sz = sz+sz)
                for (int lo = 0; lo < N-sz; lo += sz+sz)
                    merge(a, mux, lo, lo+sz-1, Math.min(lo+sz+sz-1, N-1));
        }
    }
    ```
3. 比较  
    这是一种较简单且不需要递归的mergesort，但与上面所提到使用递归（top-down）的mergesort相比，速度减慢了10%。

### Sorting Complexity
1. 计算复杂度  
    计算复杂度是用来研究算法效率的，具体包括以下概念：  
    计算模型：允许的操作；  
    代价模型：操作的次数；  
    上界：存在一些算法保证该问题的最大操作次数不会超过上界；  
    下界：对于所有算法均不会低于下界；  
    最佳算法：上界与下界相同，且该算法恰好达到上界（或下界）。
2. 排序复杂度  
    计算模型：决策树  
    代价模型：比较的次数  
    上界：mergesort的 ~ $N\lg N$  
    下界与最佳算法：需探究  
3. 决策树  
    比较a、b、c三个数的大小是很常见的决策树。
4. 排序的下界
    * 任何以比较为基础的排序算法，在最坏情况下至少要使用$\lg (N!)$ ~ $N\lg N$次比较。理由如下：  
    将最坏情况表示成一个高为$h$的决策树，则该二叉树的第$h$最多有$2^h$个子节点，而含$N$个元素的数组至少有$N!$的不同排序，即最少有$N!$个子节点，因此有 $2^h\ge N!$，即$h\ge\lg (N!)$ ~ $N\lg N$。
5. 总结  
    * 由此可知，排序算法的上界和下界均为 ~ $N\lg N$，则mergesort为最佳算法。
    * 在比较次数上，mergesort是最佳的，但在空间使用上并不是。
6. 关于下界的一些说明
    * 若算法已知下列信息，则$N\lg N$的下界可能不再成立：  
    部分排序数列：比较次数取决于起始顺序，此时可能不再需要$N\lg N$次比较。如对一个有序数组，insertion sort只需$N-1$次比较。  
    带有重复值：比较次数取决于重复值的分布，此时可能不再需要$N\lg N$次比较。如quicksort。  
    数组值具有数字特征：如radix sorts。

### Comparators
1. Comparator接口
    ```java
    public interface Comparator<Key>
        int compare(Key v, Key w)
    ```
2. 用于insertion sort的Comparator接口
    ```java
    publi static void sort(Object[] a, Comparator)
    {
        int N = a.length;
        for (int i = 0; i < N; i++)
            for (int j = i; j > 0 && less(comparator, a[i], a[i-1]); j--)
                exch(a, j, j-1);
    }

    private static boolean less(Comparator c, Object v, Object w)
    {
        return c.compare(v, w) < 0;
    }

    private static void exch(Object[] a, int i, int j)
    {
        Object swap = a[i];
        a[i] = a[j];
        a[j] = swap;
    }
    ```
3. Comparator接口的java实现(举例)  
    定义一个内嵌的类来实现Comparator接口，并实现compare()方法。
    ```java
    public class Student
    {
        public static final Comparator<Student> BY_NAME = new ByName();
        public static final Comparator<Student> BY_SECTION = new BySection();
        private final String name;
        private final int section;
        ...

        private static class ByName implements Comparator<Student>
        {
            public int compare(Student v, Student w)
            {
                return v.name.compareTo(w.name);
            }
        }

        private static class BySection implements Comparator<Student>
        {
            public int compare(Student v, Student w)
            {
                return v.section - w.section;
            }
        }
    }
    ```
4. 凸包中根据极角对点进行排列
    * 计算极角可用atan2()计算，但使用三角函数的代价较大。
    * 判断两点极角大小的基本思想  
    若$q_1$在$p$上面且$q_2$在$p$下面，则$q_1$的极角小；  
    若$q_1$在$p$下面且$q_2$在$p$上面，则$q_1$的极角大；  
    若$q_1$和$q_2$同时在$p$的上面或下面，则根据ccw的返回结果判定哪个点的极角大。
5. 判断极角的java实现
    ```java
    public class Point2D
    {
        public final Comparator<Point2D> POLAR_ORDER = new PolarOrder();
        private final double x, y;
        ...

        private static int ccw(Point2D a, Point2D b, Point2D c)
        { /* as before */}

        private class PolarOrder implements Comparator<Point2D>
        {
            public int compare(Point2D q1, Point2D q2)
            {
                double dy1 = q1.y - y;
                double dy2 = q2.y - y;

                if (dy1 == 0 && dy2 == 0) {...}
                else if (dy1 >= 0 && dy2 < 0) return -1;
                else if (dy1 <= 0 && dy2 > 0) return +1;
                else return -ccw(Point2D.this, q1, q2);
            }
        }
    }
    ```

### Stability
1. 定义  
    一个具有稳定性的排序算法是指当排序指标相同时，将保留其相对位置。（即在第二次排序后，若键值相同，则会保持第一次排序的位置。）
2. 各排序算法的稳定性
    * insertion sort  
    稳定，因为具有相同值的项不会越过去。
    * selection sort  
    不稳定，长距离的移动会导致越过具有相同值的项。
    * shellsort  
    不稳定，同样具有长距离移动。
    * mergesort  
    稳定，当两项的值相等时，总是取左半边的值，就能保持稳定。
3. 综上，mergesort是一个既快速又稳定的优秀算法。
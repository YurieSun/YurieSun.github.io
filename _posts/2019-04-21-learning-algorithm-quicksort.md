---
layout: post
title: 算法笔记（六）
description: "学习资料：algorithm (Princeton University)"
keyword: test
catogery: learning
tag: [algorithm]
---

## Quicksort

### Quicksort
1. 基本思想  
    对数组进行洗牌；将数组分成两部分，使得对于某些j，a[j]是安书序排列的，即j的左边不会有比它大的，j的右边不会有比它小的；递归对每部分进行排序。
2. partition的java实现
    ```java
    private static int partition(Comparable[] a, int lo, int hi)
    {
        int i = lo, j = hi+1;
        while(true)
        {
            while (less(a[++i], a[lo]))
                if (i == hi) break;
            
            while (less(a[lo], a[--j]))
                if (j == lo) break;
            //以a[lo]为比较值，从左向右找到比a[lo]大的值，从右向左找到比a[lo]小的值
            if (i >= j) break;//若扫描交叉了则停止
            exch(a, i, j);//交换比a[lo]大和比a[lo]小的值的位置
        }

        exch(a, lo, j);//最后将比较值与a[j]换位置
        return j;
    }
    ```
3. quicksort的java实现
    ```java
    public class Quick
    {
        private static int partition(Comparable[] a, int lo, int hi)
        { /* as before */ }

        public static void sort(Comparable[] a)
        {
            StdRandom.shuffle(a);//随机洗牌可保证性能不会下降
            sort(a, 0, a.length - 1);
        }

        private static void sort(Comparable[] a, int lo, int hi)
        {
            if (hi <= lo) return;
            int j = partiiton(a, lo, hi);
            sort(a, lo, j-1);
            sort(a, j+1, hi);
        }
    }
    ```
4. 实现中的细节
    * in-place的partition：增加一个数组可使partition更简单且稳定，但不值得增加这个花销。
    * 终止循环：如何判断指针是否交叉比看上去要棘手。
    * 到达边界的终止条件：判断(j == lo)是多余的，因为当j == lo时，less(a[lo], a[j])条件不满足，会自动跳出循环；但(i == high)是必要的。
    * 保持数组的随机性：随机洗牌可以保证性能。
    * 出现相同值：当出现重复值时，最好在与partition的比较值相同的值上停止。
5. 数学分析
    * 最好情况  
    最好情况是比较值左右分别是比它小和比它大的值，因此只有比较值的位置交换，且交换后比较值正好处在各数组的中间，此时的比较次数是 ~ $N\lg N$。
    * 最坏情况  
    最坏情况是数组完全有序时，从右向左扫描时为找到比比较值小的元素，必须扫描整个数组，此时的比较次数是 ~ $\frac{1}{2}N^2$。
    * 平均情况  
    对于长度为$N$的数组，quicksort的平均比较次数$C_N$是 ~ $2N\lg N$（交换次数是 ~ $\frac{1}{3}N\ln N$）。  
    $C_N$满足$C_0=C_1=0$，且当$N\ge 2$时，有：  
    $C_N=(N+1)+(\frac{C_0+C_{N-1}}{N})+\frac{C_1+C_{N-2}}{N}+...+(\frac{C_{N-1}+C_0}{N})$  
    两边同乘$N$，得  
    $NC_N=N(N+1)+2(C_0+C_1+...+C_{N-1})$  
    减去第$N-1$项，得  
    $NC_N-(N-1)C_{N-1}=2N+2C_{N-1}$  
    两边同除以$N(N+1)$，得  
    $\frac{C_N}{N+1}=\frac{C_{N-1}}{N}+\frac{2}{N+1}$  
    根据递推关系，得  
    $$\begin{aligned}
    \frac{C_N}{N+1}&=\frac{C_{N-1}}{N}+\frac{2}{N+1}\\&=\frac{C_{N-2}}{N-1}+\frac{2}{N}+\frac{2}{N+1}\\&=\frac{2}{3}+\frac{2}{4}+\frac{2}{5}+...+\frac{2}{N+1}
    \end{aligned}$$  
    可将求和转换成积分，得  
    $$\begin{aligned}
    C_N&=2(N+1)(\frac{1}{3}+\frac{1}{4}+\frac{1}{5}+...+\frac{1}{N+1})\\&\sim 2(N+1)\int_3^{N+1}\frac{1}{x}dx
    \end{aligned}$$  
    最终结果为$C_N\sim 2(N+1)\ln N\approx 1.39N\lg N$
6. 性能特性的总结
    * 最坏情况  
    比较次数为$N+(N-1)+(N-2)+...+1\sim \frac{1}{2}N^2$，即平方关系。
    * 平均情况  
    比较次数为 $\sim 1.39N\lg N$，和mergesort相比增加了39%，但实际上比mergesort更快，因为数据的移动更少。
    * 随机洗牌  
    是为了避免最坏情况的出现，这是上述数学模型的基础，且可以被实验验证。  
    * 注意事项  
    许多其他实现会在以下情况时出现平方关系：数组完全顺序或逆序，有许多重复项（即使已随机洗牌）。
7. 性质
    * quicksort是in-place的排序算法，partition使用的额外空间是一定的，而递归深度与洗牌后的数组顺序有关，大概率使用对数的额外空间。
    * quicksort不稳定，因为partition中元素存在大距离的移动。
7. 提高效率的方法
    * 小数组采用插入排序，因为小数组会有许多系统开销，Cutoff $\approx 10$。
    ```java
    private static void sort(Comparable[] a, int lo, int hi)
    {
        if (hi <= lo + CUTOFF - 1)
        {
            Insertion.sort(a, lo, hi);
            return;
        }
        int j = partiton(a, lo, hi);
        sort(a, lo, j-1);
        sort(a, j+1, hi)
    }
    ```
    * 选取数组开头、结尾和中间三个数的中间值作为比较元素，而不是使用开头的随机元素。若选择四个及以上的中间值，花销较大，不值得这样做。
    ```java
    private static void sort(Comparable[] a, int lo, int hi)
    {
        if (hi <= lo) return;

        int m = medianOf3(a, lo, lo + (hi-lo)/2, hi)
        swap(a, lo, m)

        int j = partition(a, lo, hi);
        sort(a, lo, j-1);
        sort(a, j+1, hi);
    }
    ```

### Selection
1. 问题描述  
    给定一个长度为N的数组，找到其中第k小的数。
2. 实现方法：quick-select  
    利用quicksort的partition，使得a[j]左边没有比它大的元素，右边没有比它小的元素，即a[j]是in-place的。
    ```java
    public static Comparable select(Comparable[] a, int k)
    {
        StdRandom.shuffle(a);
        int lo = 0, hi = a.length - 1;
        while (hi >lo)
        {
            int j = partition(a, lo, hi);
            if (j < k) lo = j + 1; 
            else if (j > k) hi = j - 1;
            else return a[k];
        }
        return a[k];
    }
    ```
3. 数学分析
    * quick-select平均使用线性时间  
    直观上，每次partition大概会在数组中间，则需要的比较次数为$N+\frac{N}{2}+\frac{N}{4}+...+1\sim2N$。  
    与quicksort类似的公式分析可得  
    $C_N=2N+2k\ln(\frac{N}{k})+2(N-k)\ln(\frac{N}{N-k})$  
    当$k=\frac{N}{2}$时，$C_N$达到最大值$(2+2\ln2)N$，即最大花销为$(2+2\ln2)N$。
    * 需要注意的是，在最坏情况下，quick-select需要$\sim \frac{1}{2}N^2$次比较，但随机洗牌能尽量避免最坏情况的出现。
4. 理论分析
    * 1973年提出一种在最坏情况下的运行时间为线性的算法，但是由于花销太大，并未在实际中使用。
    * 因此，最坏情况下存在线性时间的算法，但仍需探索可实际运用的算法。而在这个算法提出之前，如果不需要将数组进行排序，quick-select是一个很好的选择。 

### Duplicate Keys
1. 通常，带有重复项的数组都具有元素多、不同项数量少的特点。若使用mergesort，比较次数在$\frac{1}{2}N\lg N$和$N\lg N$之间；若使用quicksort，且不在相同项时停下，则会使用平方的比较次数，许多课本上和系统实现中仍存在这个问题。
2. 三分法（3-way partiton）
    * 基本思想  
    将数组划分成三部分：在下标为lt和gt之间的元素和比较元素v相等，在lt左边的数比v小，在gt右边的数比v大。
    * java实现
    ```java
    private static void sort(Comparable[] a, int lo, int hi)
    {
        if (hi <= lo) return;
        int lt = lo, gt = hi;
        Comparable v = a[lo];
        int i = lo;
        while (i <= gt)
        {
            int cmp = a[i].compareTo(v);
            if (cmp < 0) exah(a, lt++, i++);
            else if (cmp > 0) exch(a, i, gt--);
            else i++;
        }

        sort(a, lo, lt - 1);
        sort(a, gt + 1, hi);
    } 
    ```
3. 下界
    * 在最坏情况下，对于有$n$个不同元素的数组，其中第$i$个出现的次数为$x_i$，任何以比较为基础的排序算法至少需要的比较次数为$\lg(\frac{N!}{x_1!x_2!...x_n!})\sim-\sum\limits_{i=1}^{n}x_i\lg\frac{x_i}{N}$。  
    当所有元素都不同时，上式结果为$N\lg N$；当所有元素都相同时，为线性。
    * 3-way quicksort是entropy-optimal的，即无论相同元素的位置是怎样的，性能总是正比于下界。
    * 随机洗牌的3-way quicksort可将运行时间从线性对数（$N\lg N$）减小到线性。

### System Sorts
1. java系统中有sort方法，只需调用即可。
    ```java
    import java.util.Arrays;

    public class StringSort
    {
        public static void main(String arg[])
        {
            String[] a = StdIn.readString());
            Array.sort(a);
            for (int i = 0; i < N; i++)
                StdOut.println(a[i]);
        }
    }
    ```
java中的sort对于基本数据类型采用quicksort，而对于对象采用mergesort。可能的原因是，对于基本类型，性能较为重要，而对于对象类型，mergesort所使用的额外空间并不是一个需要担心的问题。
2. 设计系统中的排序
    * 基本算法是quicksort，并加上小数组的insertion sort、3-way partition及选择比较元素上的优化方法。其中，对于小数组，采用中间位置的数作为比较值；对于中等数组，采用最左、中间、最右中的中间值最为比较值；对于大树组，采用Tukey's ninther方法。以上的排序算法以广泛运用于C、C++、Java 6中。
    * Tukey's ninther方法  
    先找到三个中间值，再选择这三个值的中间值作为比较值，相当于以9个数的中间作为比较值，更接近真正的划分值。这种方法每次最多需要12次比较。且比随机洗牌可以更好地划分数组，花销也较少。
    * Java的sort方法是否solid？  
    存在一个数组会使得溢出函数调用栈，从而使程序需要平方时间，这是由于没有随机洗牌造成的。因此，随机洗牌对于性能保证是非常重要的。
3. 排序算法的选择  
    有许多的排序算法供我们选择，应该从我们对算法有哪些需求进行选择，如：是否稳定、是否含有相同项、是否需要性能保证等。
4. 排序算法的总结  
||in-place|stable|worst|average|best|remarks|
|:-:|:-:|:-:|:-:|:-:|:-:|:--:|
|selection|$\surd$||$\frac{N^2}{2}$|$\frac{N^2}{2}$|$\frac{N^2}{2}$|$N$ exchanges|
|insertion|$\surd$|$\surd$|$\frac{N^2}{2}$|$\frac{N^2}{4}$|$N$|use for small $N$ or partially ordered|
|shell|$\surd$||?|?|$N$|tight code, subquadratic|
|merge||$\surd$|$N\lg N$|$N\lg N$|$N\lg N$|$N\lg N$ guarantee, stable|
|quick|$\surd$||$\frac{N^2}{2}$|$2N\lg N$|$N\lg N$|$N\lg N$ probabilistic guarentee fastest in practice|
|3-way quick|$\surd$||$\frac{N^2}{2}$|$2N\lg N$|$N$|improves quicksort in presence of duplicate keys|
|???|$\surd$|$\surd$|$N\lg N$|$N\lg N$|$N$|holy sorting grail|

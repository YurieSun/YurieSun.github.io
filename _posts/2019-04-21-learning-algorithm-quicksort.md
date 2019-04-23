---
layout: post
title: 算法笔记（六）
description: "学习资料：algorithm (Princeton University)"
keyword: test
catogery: learning
tag: [algorithm]
---

## Quicksort
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

### Quicksort

### Selection

### Duplicate Keys

### System Sorts
---
layout: post
title: "算法笔记（二）"
description: "学习资料：algorithm (Princeton University)"
keywords: test
category: Learning
tags: [algorithm]
---


## 算法分析

### Introduction  
1. 分析算法的目的
* 预测性能
* 比较算法
* 提供保障
* 理解理论基础
* 避免性能bug（最重要）
2. 算法分析的科学方法  
（1）观察自然世界中的一些特征；  
（2）提出与观察一致的假设模型；
（3）利用假设提出预测事件；  
（4）通过进一步观察证实假设；  
（5）重复验证直到假设与观察相一致。

### Observation
1. 3-sum 问题描述  
给定N个离散整数，有多少由三个整数形成的组合其和为0？
2. 简单粗暴的算法（使用三重循环）
```java
public class ThreeSum
{
    public static int count(int[] a)
    {
        int N = a.length;
        int count = 0;
        for (int i = 0; i < N; i++)
            for (int j = i+1; j < N; j++)
                for (int k = j+1; k < N; k++)
                    if (a[i] + a[j] + a[k] == 0)
                        count++;
        return count;
    }

    public static void main(String[] args)
    {
        int[] a = In.readInts(args[0]);//In.readInts()表示从输入流中读取整数
        StdOut.printIn(count(a));
    }
}
```
3. 测量运行时间  
* 利用java的计时函数
```java
public static void main(String[] args)
{
    int[] a = In.readInts(args[0]);
    Stopwatch stopwatch = new Stopwatch();
    StdOut.printIn(ThreeSum.count(a));
    double time = stopwatch.elapsedTime();
}
```
* 法一：运行实验可得时间$T(N)$与输入大小N的关系$T(N) = aN^b$，通过log-log图可获得系数a、b，近似为立方关系。  
法二：成倍增加N的大小，并测得$T(N)$，得到时间的比值ratio，而随着持续计算，lg ratio会收敛于系数b，从而得到与时间的关系，再通过某一点得到a的值；可进行如下推导：  
由$T(N) = aN^b$得$\lg T(N)=\lg a+b\lg N$，则$lg\frac{T(2N)}{T(N)}=b\lg\frac{2N}{N}$，即$b=\lg\frac{T(2N)}{T(N)}=\lg ratio$。
4. 实验算法学 
* 与系统无关的因素（决定了指数b的值）  
算法、输入数据
* 与系统相关的因素（决定了系数a的值）  
硬件：CPU, memory, cache, ...  
软件：compiler, interpreter, garbage collector, ...  
系统：OS, network, other apps, ...

### Mathematical Models
精确计算运行时间非常麻烦，因此可以进行简化。
1. 简化方法
* cost model  
使用一些基本操作估算运行时间，如访问数组。
* tlide notation  
忽略低阶项。
2. 估计离散求和的项
* 对离散求和的项进行积分，得到其高阶项。
* 举例  
$1+2+...+N$　　　　　$\sum\limits_{i=1}^N i$ ~ $\int_{x=1}^N x dx$ ~ $\frac{1}{2}N^2$
\\
$1^k+2^k+...+N^k$　　　　　$\sum\limits_{i=1}^N i^k$ ~ $\int_{x=1}^N x^k dx$ ~ $\frac{1}{k+1}N^{k+1}$
\\
$1+\frac{1}{2}+\frac{1}{3}+...+\frac{1}{N}$　　　　　$\sum\limits_{i=1}^N \frac{1}{i}$ ~ $\int_{x=1}^N \frac{1}{x}dx=\ln N$
\\
3-sum triple loop　　　　　$\sum\limits_{i=1}^N\sum\limits_{j=i}^N\sum\limits_{k=j}^N 1$ ~ $\int_{x=1}^N\int_{y=x}^N\int_{z=y}^N dzdydx$ ~ $\frac{1}{6}N^3$

3. 总结  
理论上，可以获得运行时间的准确数学模型，但在实际应用中，公式较复杂，因此采取简化模型，可快速得到大致运行时间。

### Order-of-Growth Classifications

|order of growth|name|description|example|$\frac{T{(2N)}}{T(N)}$|  
|:-: | :-:| :--: | :-: | :-: | :-:|  
|1|constant|statement|add two numbers|1|
|$log N$|logarithmic|divide in half|binary search|~ 1|
|N|linear|loop|find the maximum|2|
|$N log N$|linearithmic|divide and conquer|mergesort|~ 2|
|$N^2$|quadratic|double loop|check all pairs|4|
|$N^3$|cubic|triple loop|check all triples|8|
|$2^N$|exponential|exhaustive search|check all subsets|$T(N)$|

### Theory of Algorithms
1. 分析类型
* 最好情况：对应下界  
由最简单输入决定，且是所有输入的一个目标。  
* 最坏情况：对应上界  
由最复杂输入决定，为所有输入提供一个保证。
* 平均情况：任一输入时的期望花销  
提供了预测性能的方法
2. 算法理论中常用的记号  

|notation|provides|example|shorthand for|used to|
|:-:|:--:|:-:|:--:|:--:|
|Big Theta|asymptotic order of growth|$\Theta(N^2)$|$\frac{1}{2}N^2$<br>$10N^2$<br>$5N^2+22N\log N+3N$<br>...|classify algorithms|
|Big Oh|$\Theta(N^2)$ and smaller|$O(N^2)$|$10N^2$<br>$100N$<br>$22N\log N+3N$<br>...|develop upper bounds|
Big Omega|$\Theta(N^2)$ and larger|$\Omega(N^2)$|$\frac{1}{2}N^2$<br>$N^5$<br>$N^3+22N\log N+3N$<br>...|develop lower bounds| 

### Memory
不同的对象所占用的内存不同。
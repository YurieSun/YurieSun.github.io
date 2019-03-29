---
layout: post
title: "算法笔记（一）"
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
* 法一：运行实验可得时间T(N)与输入大小N的关系T(N) = a$N^b$，通过log-log图可获得系数a、b，近似为立方关系。  
法二：成倍增加N的大小，并测得T(N)，得到时间的比值ratio，而随着持续计算，lg ratio会收敛于系数b，从而得到与时间的关系，再通过某一点得到a的值；可进行如下推导：  
由T(N) = a$N^b$得lg T(N)=lg a+b lg N，则lg$\frac{T(2N)}{T(N)}$=b lg$\{2N}{N}$，即b=lg$\frac{T(2N)}{T(N)}$=lg ratio。
4. 实验算法学 
* 与系统无关的因素（决定了指数b的值）  
算法、输入数据
* 与系统相关的因素（决定了系数a的值）  
硬件：CPU, memory, cache, ...  
软件：compiler, interpreter, garbage collector, ...  
系统：OS, network, other apps, ...

### Mathematical Models

### Order-of-Growth Classifications

### Theory of Algorithms

### Memory
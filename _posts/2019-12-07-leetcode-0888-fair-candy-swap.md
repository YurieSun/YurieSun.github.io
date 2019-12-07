---
layout: post
title: LeetCode 0888 题解
description: "公平的糖果交换"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[公平的糖果交换](https://leetcode-cn.com/problems/fair-candy-swap/)

### 题解
1. 法一：分别计算两人糖果总数，它们差值的一半即为所要交换糖果之差；然后同时遍历两个数组，找到满足该差值的两个数即可。
```java
class Solution {
    public int[] fairCandySwap(int[] A, int[] B) {
        int sumA = 0, sumB = 0;
        for(int x : A)
            sumA += x;
        for(int x : B)
            sumB += x;
        for(int i = 0; i < A.length; i++)
            for(int j = 0; j < B.length; j++)
                if(A[i] - B[j] == (sumA -sumB)/2)
                	return new int[] {A[i], B[j]};
        return null;
    }
}
```
* 法二：首先仍然计算需交换糖果之差；然后将数组A的元素装入`set`中，再遍历数组B，以找到与数组元素具有所计算差值的元素。
```java
class Solution {
    public int[] fairCandySwap(int[] A, int[] B) {
        int sumA = 0, sumB = 0;
        Set<Integer> set = new HashSet<>();
        for(int x : A){
            sumA += x;
            set.add(x);
        }       
        for(int x : B)
            sumB += x;
        int diff = (sumA -sumB)/2;
        for(int x : B)
            if(set.contains(x + diff) )
                return new int[] {x + diff, x};
        return null;
    }
}
```
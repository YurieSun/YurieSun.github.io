---
layout: post
title: LeetCode 0667 题解
description: "优美的排列 II"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[优美的排列 II](https://leetcode-cn.com/problems/beautiful-arrangement-ii/)

### 题解
1. 法一：需要构造出$k$个不同的差值，则将从$1$到$k+1$的序列按一定规律放在数组的前$k+1$个位置，而$k+2$之后的序列则按顺序存放。前$k+1$个位置存放的规律为：偶数下标中存放从$1$开始递增的序列，奇数下标中存放从$k+1$开始递减的序列，将形成【$1$, $k+1$, $2$, $k$, ..., $\frac{k}{2}$, $\frac{k}{2}+1$】的序列。可以看出，它们所形成的差值范围为$1$到$k$，正好有$k$个；之后的序列之间的差值均为1。而在交界处的差值为$(k+2)-(\frac{k}{2}+1)=\frac{k}{2}+1$；在$k\ge2$时，这个值将满足$\frac{k}{2}+1\ge k$，因此可以保证差值个数仍为$k$。
```java
class Solution {
    public int[] constructArray(int n, int k) {
        int[] res = new int[n];
        int startK = k + 1, start = 1;
        for (int i = 0; i < k + 1; i += 2) {
            res[i] = start++;
        }
        for (int i = 1; i < k + 1; i += 2) {
            res[i] = startK--;
        }
        for (int i = k + 1; i < n; i++) {
            res[i] = i + 1;
        }
        return res;
    }
}
```
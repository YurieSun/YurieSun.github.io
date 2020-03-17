---
layout: post
title: LeetCode 0295 题解
description: "数据流的中位数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[数据流的中位数](https://leetcode-cn.com/problems/find-median-from-data-stream/)

### 题解
1. 法一（排序）：每次需要输出中位数时，对数组进行排序。时间复杂度为 $O(n\lg n)$。
```java
class MedianFinder {
    private List<Integer> list;
    //initialize your data structure here.
    public MedianFinder() {
        list = new ArrayList<>();
    }
    public void addNum(int num) {
        list.add(num);
    }
    public double findMedian() {
        Collections.sort(list);
        int N = list.size();
        if ((N & 1) == 0)
            return (double) (list.get((N - 2) / 2) + list.get(N / 2)) / 2;
        else
            return (double) list.get((N - 1) / 2);
    }
}
```
